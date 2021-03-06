# [Ant's Game Development Blog](http://antskilton.github.io)

Just some notes for me on what I've done on making this blog for future reference. [Jekyll installation](https://jekyllrb.com/docs/installation/) if needed. Ive also set this custom blog up to work in dark mode by default and works with the Chrome override enabled too.

## Setting up this blog on a new machine

1. Install VS Code, login, pull this repo to your Git folder. I'd suggest putting it on `C:/Git` otherwise you may get issues
2. [Install the latest Ruby](https://rubyinstaller.org/downloads/)
3. In git bash run `gem install bundler` to get bundler
4. Then run `bundle install` and `bundle update` to get the latest gems from this repo's gemfile
5. If you have the x64 version of event machine running, you'll want to `gem uninstall eventmachine` and then `gem install eventmachine --platform rubygem install eventmachine --platform ruby` so it uses the right version
6. Reboot VSCode if needed
7. View markdown file previews (with Markdown in all one extension with `Ctrl + Shift + V`)

## Testing Offline

1. Edit the `_config.yml` and comment remote theme and uncomment theme.
2. For CSS, I've had the best luck in editing `_variables.scss` for fonts, `ant.scss` for colours (custom skin) and `_pages.scss` for dyanamic em scaling. I found this to work wheras the css/main.scss override file wasn't behaving on Github pages as it's based off a remote theme for that to work. The explicit ovverides seem to do the job.
3. Run `bundle exec jekyll s --livereload` which outputs to <http://localhost:4000>
4. Switch back the remote theme in `config.yml` before you push so that GitHub pages knows how to handle it all.

## Credits

This is a custom adapted Jekyll theme originally from [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/). Hosted on Github pages.
