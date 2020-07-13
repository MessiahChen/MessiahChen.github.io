# Mingjie Chen's Blog
> A blog of whatever I want to write.

## Local Preview Setup

```bash
$ brew install ruby

$ gem install jekyll bundler

# cd the directory of this directory.
$ jekyll new . --force

# installed dependencies in the Gemfile:
$ bundle install 

$ bundle exec jekyll serve  # alternatively, npm start
```
Now visit [localhost: 4000](127.0.0.1:4000) for preview.