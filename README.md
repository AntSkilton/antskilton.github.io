# Ant's Game Developer Blog

Just some notes for me on what I've done on making this blog for future reference. [Jekyll installation](https://jekyllrb.com/docs/installation/) if needed.

## Setting up this blog on a new machine

1. [Install the latest Ruby](https://rubyinstaller.org/downloads/)
2. Reboot VSCode
3. Pull repo somewhere sensible, use [Fork](https://git-fork.com/) for git client

## Rebuilding the blog

Run `jekyll s` to build (like `jekyll build` which outputs it to "_site") and it updates <http://localhost:4000> (which makes a local server) automatically in the session.

You can have live reloading of the page with: <br>`$ jekyll serve --livereload`

However you'll need to fix an issue with the event machine gem by running this before hand:<br>
`gem uninstall eventmachine` <br>
`gem install eventmachine --platform ruby`
