# [Ant's Game Development Blog](http://antskilton.github.io)

Just some notes for me on what I've done on making this blog for future reference. [Jekyll installation](https://jekyllrb.com/docs/installation/) if needed. Ive also set this custom blog up to work in dark mode by default and works with the Chrome override enabled too.

## Setting up this blog on a new machine

1. [Install the latest Ruby](https://rubyinstaller.org/downloads/)
2. Reboot VSCode
3. Pull repo somewhere sensible, use [Fork](https://git-fork.com/) for git client

## Testing Offline

1. Edit the `_config.yml` and comment remote theme and uncomment theme.
2. For CSS, I've had the best luck in editing `_variables.scss` for fonts, `ant.scss` for colours (custom skin) and `_pages.scss` for dyanamic em scaling. I found this to work wheras the css/main.scss override file wasn't behaving on Github pages as it's based off a remote theme for that to work. The explicit ovverides seem to do the job.
3. Run `bundle exec jekyll s --liverelead` which outputs to <http://localhost:4000>
4. Switch back the remote theme in `config.yml` before you push so that GitHub pages knows how to handle it all.

## Credits

This is a custom adapted Jekyll theme originally from [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/). Hosted on Github pages.
