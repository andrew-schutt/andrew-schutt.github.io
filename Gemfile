source "https://rubygems.org"

# Modern Jekyll, built and deployed via GitHub Actions (see .github/workflows/jekyll.yml).
# Note: this repo no longer uses the `github-pages` gem, which pinned Jekyll 3.9 / Ruby < 4.0.
gem "jekyll", "~> 4.3"

group :jekyll_plugins do
  gem "jekyll-remote-theme"
  gem "jekyll-feed", "~> 0.17"
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-target-blank"
end

# Modern Ruby no longer ships webrick in the stdlib; needed for `jekyll serve`.
gem "webrick", "~> 1.9"

# Windows and JRuby do not include zoneinfo files, so bundle the tzinfo-data gem.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end
