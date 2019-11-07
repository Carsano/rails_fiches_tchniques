toutes les gems: https://rubygems.org/

## installation de gem

==> gem install gem_name

OU 

==> dans Gemfile + $ bundle install

## list des gems installées

$ gem list

## Gemfile

source "https://rubygems.org"

git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '2.5.1'
gem 'rspec'
gem 'pry'
gem 'rubocop', '~> 0.57.2'
gem 'nokogiri'
gem 'rubysl-open-uri', '~> 2.0'
gem 'watir'
gem 'selenium-webdriver'
gem 'csv'
gem 'bcrypt', '~> 3.1.7'
gem 'mini_magik'
gem 'devise'
gem 'mail_form'
gem 'dotenv-rails', groups: [:development, :test]
gem "font-awesome-rails"
gem "aws-sdk-s3", require: false