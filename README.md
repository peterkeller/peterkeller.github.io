# Posts from Blogger to Github using Jekyll

## Need help? 

See:

 - http://jekyllrb.com/docs/configuration/
 - https://help.github.com/articles/using-jekyll-with-pages/
 - http://jekyllbootstrap.com/
 - http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages/
 - https://github.com/barryclark/jekyll-now
 - http://octopress.org/

## Install jekyll

?

## Run jekyll locally

Run:

    bundle exec jekyll serve

Or only:

    jekyll serve

See blog locally:

 - http://localhost:4000/
 - http://localhost:4000/2014/12/10/why-not.html


## Update GIT posts to Github

    git add --all
    git commit -m"My commit"
    git push

See blog worldwide:

 - http://peterkeller.github.io/


## Install Theme

    rake theme:install git="git://github.com/sodabrew/theme-dinky.git"


## Import from Blogger 

See http://import.jekyllrb.com/docs/blogger/

### Export from Blogger

Export Blogger posts see https://support.google.com/blogger/answer/41387?rd=1

 1. Sign in to Blogger.
 2. Select the blog to export.
 3. In the left menu, click Settings > Other.
 4. In the "Blog tools" section, click Export blog > Download blog.

File export will be downloaded to /Users/peter/Downloads. File is
named `blog-MM-DD-YYYY.xml`, e.g. `blog-01-16-2016.xml`.

### Read export and create/update posts and drafts

Install tool:

    sudo gem install jekyll-import

Read the posts (adjust export file name). 

Change directory:

    cd peterkeller.github.io/

Copy all in one:

    ruby -rubygems -e 'require "jekyll-import";   
    JekyllImport::Importers::Blogger.run({
      "source"                => "/Users/peter/Downloads/blog-01-16-2016.xml",
      "no-blogger-info"       => false, # not to leave blogger-URL info (id and old URL) in the front matter
      "replace-internal-link" => false, # replace internal links using the post_url liquid tag.
    })'

The contents of `_drafts/` and `_posts/` are now updated. Note,
that you may now have duplicates `*.html` and `*.md`. 