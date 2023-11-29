source "https://rubygems.org"
ruby '2.7.4'
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     jekyll serve -oli
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!

# If you have any plugins, put them here!
# Refer GitHub Pages version dependencies from https://pages.github.com/versions/
gem "github-pages", "228", group: :jekyll_plugins

group :jekyll_plugins do
  gem 'json'
  gem 'jekyll-feed'
  gem 'jekyll-json-feed'
  gem 'jekyll-sitemap'
  gem 'jekyll-redirect-from'
  gem 'jekyll-include-cache'
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
