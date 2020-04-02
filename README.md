# Jekyll
## System install (one-time only)
```bash
# ruby install, not sure it's always needed, but purging existing install
sudo apt purge ruby*
sudo apt-get install ruby-full build-essential zlib1g-dev

# set GEM_HOME for the user (do not use system path)
# https://jekyllrb.com/docs/installation/ubuntu/
echo 'export GEM_HOME=~/.ruby/' >> ~/.bashrc
echo 'export PATH="$PATH:~/.ruby/bin"' >> ~/.bashrc
source .bashrc

# Install bundler that is used for dependencies
# always run bundle exec CMD
# installing jekyll here is just for running jekyll new site
gem install jekyll bundler
```

## Project create and dev
```bash
# create site
jekyll new abc
cd abc
```
Use a theme available as gem, here just-the-docs
**Gemfile**
```gem
source "https://rubygems.org"
gem "jekyll"
gem "just-the-docs"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end
```
Set in **_config.yml**
```yml
theme: just-the-docs
```

```bash
# apply changes in Gemfile, here just-the-docs
bundle
# rebuild static site
bundle exec jekyll serve
```

## Deploy on github
TODO github-pages

# Add content
Checkout the doc of [just-the-docs](https://pmarsceill.github.io/just-the-docs/)
TODO

