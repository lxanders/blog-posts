{
  title: 'Getting started with technical blogging',
  date: '2014-09-07',
  author: 'Alexander Schmidt'
}

Why blog
--------

As you might already know, blogging is a great way to share your experiences and thoughts. From my point of view, technical writing is especially great. It offers an easy way to help others and profit from your hard learned lessons. This comes with an additional benefit for the writer: By teaching others or writing things in detail for others the writer can get a new perspective which things might be hard to get or which he doesn't understand totally himself. Writing them down in detail helps the blogger to remember the things he is writing about. Last but not least you can use your own blogs if you run into the same problem once more.

Some days ago I had the pleasure to attend the MunichJS meetup -- a meeting for JavaScript enthusiasts in Munich, Germany. One of the speakers was Patrick Mulder (@mulpat) who is himself an active blogger, a software engineer and web developer. His topic was technical writing and blogging and he inspired me, to write something myself (thanks for that, Patrick!). His talk was focussed on writing itself but he mentioned some interesting tools that get you started with your own blog. You can find the [slides to his talk](https://speakerdeck.com/mulderp/blogging-and-writing-about-javascript) here and visit his really [interesting blog](http://thinkingonthinking.com/).

Blogging engines in general
---------------------------

Being a software engineer myself, I try to keep things simple ([KISS principle](http://en.wikipedia.org/wiki/KISS_principle)). Many blogging engines are out there that advertise themselve as especially easy to use but the ones I know have some points in common that I don't like particularly.

Well known blogging engines like [wordpress.com](https://wordpress.com/) let you setup your blog very fast. They offer things like [WYSIWYG](http://en.wikipedia.org/wiki/WYSIWYG) and a rich feature set. Unfortunately this comes at the cost of not having your blog posts in a standard format that can be opened in any text editor. If you want to host your blog yourself you can use the specific wordpress version for this from [wordpress.org](http://wordpress.org/). It is quite pluggable and extensible but are not very easy to customize and all your time you put into changing the blog to your needs is invane once you want to change to another blogging engine. Other problems are that through their rich feature set they tend to be the target of security breaches.

For me a blogging system should 

- be easy to setup
- don't require special technology on the server side -- a http web server should be enough
- use a standard syntax for writing your blog articles so you can use them in other blogging engines or documents
- bring no limitations how to backup your blog posts in a source format that can be reapplied without any problems later on another blogging engine
- be easily customizable with standard web technologies
- make deploying the blog posts easy
- offer an easy way to delete or add blog posts

These points can be achieved by using so called static site generators.

Blogging engines I tried
------------------------

In the last months I tested various static site generators:

- [jekyll](http://jekyllrb.com/)
- [metalsmith](http://www.metalsmith.io/)
- [hexo](http://hexo.io/)

All of the were quite easy to use but didn't really convince me. While e.g. hexo was to bloated, metalsmith was not easy enough to use in the beginning and jekyll had a displeasing configuration.

Patrick mentioned in his talk I mentioned above [Cabin](http://www.cabinjs.com/). After a short test I wanted to try it some time as my blogging engine. The tool fulfills all the points I mentioned above. Some of the points I like especially:
- Syntax highlighting works out of the box
- Cabin is installable as [node](http://nodejs.org/) module
- It is quite lightweight
- It offers a [grunt](http://gruntjs.com/) configuration that offers a watcher that automatically rebuilds your static blog content after changes (among other tasks)
- Deployment is quite easy by configuring and using the grunt task that is present for it
- The contained themes are pleasing and simple

Setting up your Cabin blog
--------------------------

Cabin has a good and simple [getting started guide](http://www.cabinjs.com/#getting-started). One point is missing from it: The installation of [compass](http://compass-style.org/) is only mentioned, not covered. Just follow the guide and before you start Cabin head to the [compass installation instructions] and install it.

You can use these commands to install the dependencies and Cabin (you will need to install node and ruby before you can do that):

```sh
npm install -g cabin grunt-cli
gem update --system && gem install compass

```

Now just follow the getting started guide further. It will get you started without any problems.

Customization
-------------

As Cabin is very lightweight, customization proves to be easy. The source code that is used for templating (using te [jade](http://jade-lang.com/) templating engine) is organized this way:

- src
-- images
-- layouts: contains the layouts for the page -- e.g. the base template `base.jade` and the post template `post.jade`
-- pages: the templates for the pages like `about` or the `archive`
-- scripts
-- styles

Save your blog posts
--------------------

As mentioned above one very important point for me is getting my blog source files backed up. Using [git](http://git-scm.com/) for this is perfect for me. I created a repository that holds my Markdown text files with the blog posts.

Adding a grunt task to copy your posts to another directory is the first step. I added the following to the `Gruntfile.js`:

```js
// [...]
// In the already existing copy task definition (look for 
// [...] copy: { dist: { files: [ [...]
// After this:
backup: {
    files: [
        {
            expand: true,
            src: ['posts/**.md'],
            dest: '../blog-posts/',
            filter: 'isFile'
        }
    ]

// Replace all occurrences of this defined copy task by copy:dist

// In the section for registering grunt tasks (look for grunt.registerTask('deploy', ['build', 'gh-pages']);
// Add the following:
grunt.registerTask('backup', ['copy:backup']);
```

Running `grunt backup` now copies my files to another directory which is actually a GitHub repository where I after that commit my changes. It would be easy to commit automatically but until now I want to keep this step manual.

From now on
-----------

Feel free to contact me if you found this post helpful, something was missing for you or unclear or if you want to tell me something is wrong.