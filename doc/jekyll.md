---
layout: page
title: Jekyll
nav_order: 3
---

* TOC
{:toc}
# System install (one-time only)
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

# Project create and dev
```bash
# create site
jekyll new abc
cd abc
```
Use a theme available as gem, here just-the-docs  
**Gemfile :**
```ruby
source "https://rubygems.org"
gem "jekyll"
gem "just-the-docs"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end
```
Set in **_config.yml :**
```yml
theme: just-the-docs
```

```bash
# apply changes in Gemfile, here just-the-docs
bundle
# rebuild static site
bundle exec jekyll serve
```

# Deploy
## framagit (using CI)
Setup CI with  
**.gitlab-ci.yml :**
```yaml
# This file is a template, and might need editing before it works on your project.
# Template project: https://gitlab.com/pages/jekyll
# Docs: https://docs.gitlab.com/ce/pages/
image: ruby:2.5

variables:
  JEKYLL_ENV: production
  LC_ALL: C.UTF-8

before_script:
  - gem install bundle
  - bundle -v
  - bundle install

pages:
  stage: deploy
  script:
    - bundle exec jekyll -v
    - bundle exec jekyll build -d public
  artifacts:
    paths:
      - public
  only:
    - framagit-pages
```

## github (github-pages)
It is usable locally as well (github-pages is a deps bundle, same as deployed on github)  
**Gemfile :**
```bash
# comment out
#gem "jekyll"
#gem "just-the-docs"

# add in Gemfile
gem "github-pages", group: :jekyll_plugins

bundle
```
**_config.yml :**
```bashrc
# replace theme: blabla with
remote_theme: pmarsceill/just-the-docs

url: "https://barchstien.github.io/abc"
```

# Theme
## just-the-docs
### custom CSS
Copy from the theme source of make a new  
**_sass/custom/custom.scss :**
```scss
# usefull examples

$body-line-height: 1.0;
$content-line-height: 1.0;
$body-heading-line-height: 1.0;

//$body-background-color: $white;
//$sidebar-color: $grey-lt-000;
//$search-background-color: $white;
//$table-background-color: $white;
//$code-background-color: $grey-lt-000;

//$body-text-color: $grey-dk-100;
//$body-heading-color: $grey-dk-300;
//$search-result-preview-color: $grey-dk-000;
//$nav-child-link-color: $grey-dk-100;
//$link-color: $purple-000;
//$btn-primary-color: $purple-100;
//$base-button-color: #f7f7f7;
```

# Add content
Checkout the doc of [just-the-docs](https://pmarsceill.github.io/just-the-docs/)  
Basically add markdown files to a folder, and look at navigation docs of the theme


