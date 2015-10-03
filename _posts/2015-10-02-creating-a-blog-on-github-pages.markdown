---
layout: post
title:  "Creating a blog with Jekyll and GitHub Pages"
date:   2015-10-02 10:41:00
categories: jekyll github blogging
---

For awhile I've been meaning to put together a tech blog so that I can share my
useful notes with the world.  For my first entry, I thought I'd document how I
set this blog up.

I'm assuming that you're not an expert on any of the tools we're using, but that
you know enough about Ruby package management (via gem), github, and GitHub, to be
dangerous.

Create your GitHub repo
=======================

This blog is hosted on [GitHub Pages](https://pages.github.com/), which is a great way to publish a simple,
minimalist blog.  There are a few things you need to do first, which I won't cover
in detail, as they're already documented well on the GitHub website.

* [Register](http://github.com/join) for a GitHub account if you don't have one already
* [Configure ssh access]( http://help.github.com/articles/generating-ssh-keys/) to your GitHub account, so you can push to your repositories from your computer
* [Create a repository](https://pages.github.com/#project-site) for your blog.  Make sure to make `gh-pages` the default branch as described in that documentation.

Create your local git repo
==========================

Once this is all set up, you can clone the blog repository to a local
repository.   I'll assume your blog repository is called
`techblog`.  If you want to call it something else, modify the examples
accordingly.

    mkdir techblog
    cd techblog
    git clone git@github.com:my-github-user/techblog.git .

Now you can try to push a new page to your site.  Create a file called
`hello.html` and add some text to it:

    hello world from github pages!

You need to commit this to your local repository, then push it to your GitHub
repository:

    cd techblog
    git add hello.html
    git commit -m"hello"
    git push

If you browse to <https://my-github-user.github.io/techblog/hello.html>, you
should see your text.

Install Jekyll
==============

Next thing you need to do is set up [Jekyll](https://jekyllrb.com).  It's a Ruby package, and so is
easiest installed via gem:

    gem install jekyll

If you get a warning similar to this:

    WARNING:  You don't have /home/jason/.gem/ruby/2.2.0/bin in your PATH,
	    gem executables will not run

Then add the specified directory to your `$PATH`

Jekyll needs a javascript runtime.  You can install therubyracer via gem as well:

    gem install therubyracer

To generate your first Jekyll site (assuming you're still in your `techblog`
directory):

    jekyll new --force .

(You need the `--force` because you already have a couple files in here.)

You can run a local test server to see how it looks:

    jekyll serve

You should see a basic site at <http://localhost:4000>

Hit `Ctrl-C` to kill the test server.  

Publish to GitHub Pages
=======================

Now let's publish it on github.io.  First you need to install the github-pages
gem that will guarantee your jekyll build environment matches GitHub Page's
environment.  

    gem install github-pages

There are a couple of changes you need to make to `_config.yml` in order for
github.io to display  your page correctly.  Modify the `baseurl:` and `url:` values as
follows:

    baseurl: "/techblog/"
    url: "http://my-github-user.github.io"

While you're editing that file, you can change any other values as you like.

After saving your `_config.yml`, you can add the new files to git, and push to
your GitHub repo:

    git add .
    git commit -m "my first jekyll"
    git push

If you browse to <https://my-github-user.github.io/techblog/>, you should see your
site.

Try to run a local test server again

    jekyll serve

When you bring up <http://localhost:4000>, you'll now find that you get a "Not
Found" error.  Since you modified baseurl in `_config.yml`, you need to add the
`--baseurl` flag when running your test server, to override the value in your
config file:

    jekyll serve --baseurl=""

You can use this local server to easily test any changes you make before
commiting and pushing to GitHub.

Add content to your blog
========================
The first change you may want to make is to change the "About" page.  To do
this, edit `about.md`.  Modify the text as desired.

Bring up the page in your local testserver and you should see that it contains your text

To create a post, create a new file in the `_posts` directory.  The filename
should be in the format `YYYY-MM-DD-my-blog-post-title.markdown`.  For example,
this post's filename is `2015-10-02-creating-a-blog-on-github-pages.markdown`.

You should place the following header at the top of the file:

    ---
    layout: post
    title:  "Welcome to Jekyll!"
    date:   2015-10-02 10:01:19
    categories: jekyll update
    ---

Change the title, date, and categories as appropriate of course.  In Jekyll
terms, this header is called the ["Front
Matter"](http://jekyllrb.com/docs/frontmatter/), and must be valid YAML. The
body of the post can be any valid
[Markdown](https://daringfireball.net/projects/markdown/).  

It's beyond the scope of this article to go into the details of Markdown, but
it's simple to figure out from browsing the
[documentation](https://daringfireball.net/projects/markdown/).

Once you've written your first post, look at your test server in your browser
again, and you'll see that a link to your post has been added to the index page.

If everything looks good, you can now publish to GitHub:

    git add .
    git commit -m "my first blog post"
    git push

If you browse again to <https://my-github-user.github.io/techblog/>, you should see your new post on the index page, and be able to read it.

Have fun!
