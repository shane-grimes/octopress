$:.unshift File.expand_path("lib", File.dirname(__FILE__)) # For use/testing when no gem is installed

# A sample Guardfile
# More info at https://github.com/guard/guard#readme
require 'octopress'

config    = Octopress::Configuration.read_configuration
js_assets = Octopress::JSAssetsManager.new

stylesheets_dir    = "assets/stylesheets"
javascripts_dir    = "assets/javascripts"

guard :compass do
  watch %r{^#{stylesheets_dir}/(.*)\.s[ac]ss$}
end

guard :shell do
  # If a config file changes, reload configuration
  watch /^_config/ do
    config    = Octopress::Configuration.read_configuration
    js_assets = Octopress::JSAssetsManager.new
    "Configuration reloaded"
  end

  # If a template file changes, trigger a Jekyll build
  watch /^#{config[:source]}\/.+\.(md|markdown|textile|html|haml|slim|xml)/ do
    Octopress::Configuration.write_configs_for_generation
    Thread.new { system 'jekyll' }
    Octopress::Configuration.remove_configs_for_generation
  end

  watch /^#{javascripts_dir}\/.+\.(js|coffee|mustache|eco|tmpl)/ do |change|
    js_assets.compile
  end
  # If a non template file changes, copy it to destination 
  watch /^#{config[:source]}\/.+\.[^(md|markdown|textile|html|haml|slim|xml)]/ do |m|
    if File.exists?(m.first)
      file = File.basename(m.first)
      path = m.first.sub /^#{config[:source]}/, "#{config[:destination]}"
      FileUtils.mkdir_p path.sub /#{file}/,''
      FileUtils.cp m.first, path
      "\nCopied #{m.first} to #{path}"
    end
  end
end

