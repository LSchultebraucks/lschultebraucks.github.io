source "https://rubygems.org"

# GitHub Pages builds this site with its own pinned dependency set. Using the
# github-pages gem locally keeps `bundle exec jekyll serve` on exactly the
# Jekyll and plugin versions that run in production.
gem "github-pages", group: :jekyll_plugins

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-sitemap"
  gem "jekyll-seo-tag"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]

# Required on Ruby 3.x, where webrick is no longer a default gem
gem "webrick", "~> 1.8"
