require 'yaml'
require 'github-pages'
require 'uri'

def test_config
  YAML.load_file('_config_test.yml')
end

task :set_env do
  ENV['DISABLE_WHITELIST']   = 'true'
end

def build_site
  sh "bundle exec jekyll build -c _config.yml,_config_test.yml"
end

task :test do
  build_site
  require 'html-proofer'
  HTMLProofer.check_directory('./_site', test_config['proofer']).run
end
