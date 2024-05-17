## Software Development Blog

### Local Development Setup

1. [Jekyll installation](https://jekyllrb.com/docs/installation/)
2. [Ruby installation (latest)](https://rubyinstaller.org/downloads/)
3. `gem install bundle` to get the Bundler gem.
4. `bundle install` / `bundle update` to get the latest gems from gemfile.
   1. If Bundler can't find the compatible gems, delete `Gemfile.lock` and try again.
5. If you have the x64 version of event machine running, run `gem uninstall eventmachine` (all versions), then `gem install eventmachine --platform rubygem install eventmachine --platform ruby` so it uses the right version of eventmachine

### Testing

1. View markdown file previews (with Markdown in all one extension with `Ctrl + Shift + V`)
2. Edit the `_config.yml` and comment `remote theme` and uncomment `theme`.
3. For CSS, I've had the best luck in editing `_variables.scss` for fonts, `ant.scss` for colours (custom skin) and `_pages.scss` for dynamic em scaling. I found this to work whereas the css/main.scss override file wasn't behaving on Github pages as it's based off a remote theme for that to work. The explicit overrides seem to do the job.
4. Run `bundle exec jekyll s --livereload` which outputs to <http://localhost:4000>
5. Switch back the remote theme in `config.yml` before you push so that GitHub pages knows how to handle it all.
6. If you get `bundle` errors, delete `Gemfile.lock` and try again.

_This is a custom theme adapted Jekyll theme originally from [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/). Hosted on Github pages._
