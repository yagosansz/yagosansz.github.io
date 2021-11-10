---
layout: post
title:  "Patrolling Your Code Base with RuboCop"
date:   2021-11-09 10:15:00 -0500
categories: ruby rails rubocop
---

RuboCop is a code style enforcer (a.k.a linter) and code formatter based on the community-driven [Ruby Style Guide](https://rubystyle.guide/).

The rules to be followed are placed in the `.rubocop.yml` file. Each rule to be enforced is a *Cop*, and it can have its behaviour changed and even desactivated if necessary. 

Other than Ruby, RuboCop also provides gems for implementing rules on various extensions such as [Rails](https://github.com/rubocop/rubocop-rails), [RSpec](https://github.com/rubocop/rubocop-rspec/).

## Setting up RuboCop

1. Add the following lines to your *Gemfile*

```ruby
group :development, :test do
  gem "rubocop" # for ruby specific rules
  gem "rubocop-rails", require: false # for rails specific rules
  gem "rubocop-rspec", require: false # for rspec specific rules
end
```

> [What does `require: false` means?](https://stackoverflow.com/a/35646481/7274495)

2. Run `bundle install` to install the newly added gems

3. Run `gem install rubocop` to install rubocop globally
  - This will help us to execute commands provided by the rubocop gem, like
  auto-formatting, running rubocop in the project.

## Adding Configuration the File(s)

You can add all rules to a **.rubocop.yml** file:

```
touch .rubocop.yml
```

Although, it is also possible to have individual files for each RuboCop specific extension, like *.rubocop-rails.yml* and *.rubocop-rspec.yml*. If you decide to follow that path, remember to inherit those files in your *.rubocop.yml*:

```yml
inherit_from:
  - '.rubocop-rails.yml'
  - '.rubocop-rspec.yml'
```

## What should go in *.rubocop.yml*?

It's pretty common for organizations to define their own custom *.rubocop.yml* file. For instance, [Shopify has their own RuboCop](https://ruby-style-guide.shopify.dev/) template that is based on the Ruby Style Guide.

I'd advise to start with a straigthforward template that you can tailor to your needs. When I was setting up my RuboCop configuration file for the first time, I used the two templates below as a guides:

1. [ChaelCodes/ConfBuddies *.rubocop.yml*](https://github.com/ChaelCodes/ConfBuddies/blob/main/.rubocop.yml)

2. [CampusCode sample *.rubocop.yml*](https://campuscode.com.br/conteudos/configurando-o-rubocop)
  + Since this one is writen in pt-BR, I've included the translated file below:

```yml
require:
  - rubocop-rails

# It's common from projects to have a *.rubocop_todo.yml* file.
# If your project does not have one, you can remove the line below.
inherit_from:
  - .rubocop_todo.yml

# Update the Ruby version to match the one being used in your project. 
# You can also include files and/or folders that should not be checked by RuboCop.
AllCops:
 TargetRubyVersion: 2.6
 Exclude:
   - 'bin/**/*'
   - 'vendor/**/*'
   - 'db/**/*'
   - 'config/**/*'
   - 'script/**/*'
   - 'spec/rails_helper.rb'

# If your project is well tested, there is no need to allow comments
# throughout out your code.
Documentation:
 Enabled: false

# This is a controverse Cop, because it can improve your project's performance.
# It will be a default configuration on Ruby 3.
# If you do not with to enable it, just remove the lines below.
Style/FrozenStringLiteralComment:
 Enabled: false

# We usually write lots of lines of code in test files.
# Therefore, it's a good idea to not set a limit for that.
Metrics/BlockLength:
 ExcludedMethods: ['describe', 'context', 'feature', 'scenario', 'let']

# This is RuboCop's standard for line length.
# Feel free to change it, if you think it's necessary.
Metrics/LineLength:
  Max: 80

# This is another controverse Cop.
# If you feel that sometimes it can be necessary to write comments in your
# code that use accents and non ascii characters, you'll need to leave that cop disabled.
AsciiComments:
  Enabled: false
```

3. [My first *.rubocop.yml* file](https://github.com/yagosansz/alexandria/blob/main/.rubocop.yml), which is still under construction!

## How to Run RuboCop?

The `rubocop` command can be run in 4 main different ways:

- Project

```
rubocop
```

- Folder

```
rubocop app/models
```

- File

```
rubocop app/models/book.rb
```

- List of file(s) and folder(s)

```
rubocop spec lib/something.rb
```

## How to Run Specific Auto-Corrections?

There are a bunch of ways to run [auto-correction commands](https://docs.rubocop.org/rubocop/usage/auto_correct.html), but as a beginner to RuboCop I've found it safer to manually fix the offenses or run the command bellow to fix the more straightforward ones.

```
rubocop --auto-correct-all --only Style/StringLiterals
```

## How to Start with RuboCop in Large Code Bases?

If you are working on a legacy code base and want to add RuboCop to it, it is a good idea to use `rubocop --auto-gen-config`. That command creates both a *.rubocop.yml* and *.rubocop_todo.yml* file, where the latter needs to be inherited on the the former, like so:

```yml
inherit_from:
  - .rubocop_todo.yml
```  

The generated *.rubocop_todo.yml* contains the offenses found in your project, which have been disabled and/or excluded, so you can fix them and then cut and paste the configuration into *.rubocop.yml* at your own pace. There is no need to move all of it, but only what you think is aligned with your organization's or team's code style.

## References

- [RuboCop](https://rubocop.org/)
- [Usando RuboCop com Ruby e Rails (pt-BR)](https://www.campuscode.com.br/conteudos/usando-rubocop-com-ruby-e-rails)
- [Beginner's Guide to RuboCop in Rails](https://prabinpoudel.com.np/articles/beginners-guide-to-rubocop-in-rails/)

## Recommended Article

- [**RuboCoping with legacy**: Bring your Ruby code up to Standard](https://evilmartians.com/chronicles/rubocoping-with-legacy-bring-your-ruby-code-up-to-standard)