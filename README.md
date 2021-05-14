# README

> CC-BY-SA

## Use Alembic Instead

This is a very simple starting point if you wish to use Alembic [as a Jekyll theme gem](https://alembic.darn.es/#as-a-jekyll-theme) or as a [GitHub Pages remote theme](https://github.com/daviddarnes/alembic-kit/tree/remote-theme) (see `remote-theme` branch).


or

**[Download the GitHub Pages kit](https://github.com/daviddarnes/alembic-kit/archive/remote-theme.zip)**

```bash
# download the kit
bundle install
bundle exec jekyll serve
```

## Former Page Cayman 

###  ksry-Ruby

```bash
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
gem sources -l
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```

### alter Minima 

> Ruby 3.0.1 Jekyll 4.2.0

```bash
gem install jekyll bundler
gem install bundler:2.2.17
# add webbrick to Gem
bundle install
bundle exec jekyll serve
```

### Preview locally

> Of course, you should install ruby first.

Here are some useful links on how to preview this page locally:

- [sof](https://stackoverflow.com/questions/47508603/running-jekyll-locally-on-a-github-pages-site-project)

- [cayman-github](https://github.com/pages-themes/cayman)

- [page-cn](https://sspai.com/post/54608#!)

`scripts/bootstrap` forked from [github.com/pages-themes/cayman](https://github.com/pages-themes/cayman)

```bash
# 
sh scripts/bootstrap
bundle exec jekyll serve
```
### TypeError(jekyll server)

[solved-issue](https://bbs.archlinux.org/viewtopic.php?id=265534)

```log
 Incremental build: disabled. Enable with --incremental
      Generating... 
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 1.383 seconds.
jekyll 3.9.0 | Error:  no implicit conversion of Hash into Integer
/home/scarlet/.rvm/gems/ruby-3.0.1/gems/pathutil-0.16.2/lib/pathutil.rb:502:in `read': no implicit conversion of Hash into Integer (TypeError)
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/pathutil-0.16.2/lib/pathutil.rb:502:in `read'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/utils/platforms.rb:75:in `proc_version'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/utils/platforms.rb:40:in `bash_on_windows?'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/commands/build.rb:77:in `watch'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/commands/build.rb:43:in `process'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:93:in `block in start'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:93:in `each'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:93:in `start'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:75:in `block (2 levels) in init_with_program'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/mercenary-0.3.6/lib/mercenary/command.rb:220:in `block in execute'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/mercenary-0.3.6/lib/mercenary/command.rb:220:in `each'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/mercenary-0.3.6/lib/mercenary/command.rb:220:in `execute'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/mercenary-0.3.6/lib/mercenary/program.rb:42:in `go'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/mercenary-0.3.6/lib/mercenary.rb:19:in `program'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/jekyll-3.9.0/exe/jekyll:15:in `<top (required)>'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/bin/jekyll:23:in `load'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/bin/jekyll:23:in `<top (required)>'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/cli/exec.rb:63:in `load'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/cli/exec.rb:63:in `kernel_load'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/cli/exec.rb:28:in `run'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/cli.rb:494:in `exec'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/vendor/thor/lib/thor/invocation.rb:127:in `invoke_command'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/vendor/thor/lib/thor.rb:392:in `dispatch'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/cli.rb:30:in `dispatch'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/vendor/thor/lib/thor/base.rb:485:in `start'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/cli.rb:24:in `start'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/exe/bundle:49:in `block in <top (required)>'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/lib/bundler/friendly_errors.rb:130:in `with_friendly_errors'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/gems/bundler-2.2.17/exe/bundle:37:in `<top (required)>'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/bin/bundle:23:in `load'
        from /home/scarlet/.rvm/gems/ruby-3.0.1/bin/bundle:23:in `<main>'
```





