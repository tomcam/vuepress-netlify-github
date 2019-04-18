![Generic biz pic](/wordpress-site-1280.jpg)
# Configuring VuePress to work with Netlify and GitHub

By Tom Campbell, creator of [VuePress Book](http://vuepressbook.com).
Find this code on [GitHub](http://github.com/tomcam/vuepress-netlify-github),
or live on [Netlify](https://vuepress-netlify-github.netlify.com/).

## An Embarrassment of riches

The modern web dev world is amazing. 
[VuePress](http://vuepress.vuejs.org/) lets you create
beautiful, high-performance static sites with ease. It integrates
modern tools like yarn into its workflow, which means that
you can use those same tools at Netlify to generate your
VuePress site and publish it for free!

VuePress converts Markdown files into HTML pages with free
built-in search (of headings levels 1-3), which means that
in case you don't like VuePress you can use your same
markdown articles in any other CMS that supports Mardkdown,
from Ghost to Jekyll to Hugo to WordPress. Restricting
the input to text that conforms to Markdown standards has
a powerful side effect: it forces you to concentrate on writing
and the structure of your articles instead of constantly reworking
site parameters or format options.

## VuePress prefers a /docs directory

[VuePress](https://v1.vuepress.vuejs.org/) likes its
text to be in a `/docs/` directory off root. If you 
play your cards right it will rewrite the `/docs/` part
out of URL. This article shows you how to set up
your workflow so VuePress, Netlify, and local preview all
know what to do in terms of transforming your 
[Markdown](https://github.github.com/gfm/)
files into a cohesive website that renders accurately
on your local machine and upon deployment to the real world by Netlify.

## Creating a GitHub repo: WATCH CAREFULLY!

Now do exactly as I say here unless you're a 
Git expert. If you're used to adding a README.md
right away when you create your repo at GitHub, for
example, resist the temptation.

### Assumptions

* This sample code lives in a directory 
located at `~/Dropbox/work`, but obviously 
it could be anywhere.

* In this example the GitHub repo name is `vuepress-netlify-github`.
You would replace this with your own repo name.

* In this example the username is mine (`tomcam`). You would
replace this with your own username.

* In this example `nvim` is the name of the [editor](https://neovim.io/)
I use to create text files. Replace this with the name
of your own editor.

## Setting up your work directories

VuePress requires a fairly deep directory structure.

* Create a directory
```sh
mkdir -p ~/Dropbox/work/vuepress-netlify-github/docs/.vuepress/public
```
The new "root" directory will of course be `/docs/`. As you probably
know the hidden `/.vuepress` directory houses not only
`config.js` but also a `public` directory for static
assets. It is treated as root in URLs.

## Creating your root README.md file

* Change to the root directory.

```sh
cd ~/Dropbox/work/vuepress-netlify-github
```

* Create a file called README.md in the `/docs/` directory.

```sh
nvim ~/Dropbox/work/vuepress-netlify-github/docs/README.md
```

* Give it some contents and save it:

```md
# Welcome

Thank you for visiting.
```

## Adding README.md to source code control

Now it's time to put README.md under source code
control and push it up to GitHub, where Netlify
can find it.

The first thing to do is to turn this directory into a git repo.

```sh
git init
```

That only needs to be done once. You'll repeat this next 
sequence frequently as you update the document.

```sh
# Each time you update a document
# you'll perform these steps.
git add docs/README.md
git commit -m "Create home page"
```
* Let Git and GitHub connect this local repository to
the one you created interactively on GitHub.

Again:

* Replace `tomcam` with your username
* Replace `vuepress-netlify-github` with the name of your project.

```sh
git remote add origin https://github.com/tomcam/vuepress-netlify-github.git
```

You won't have to repeat the previous step.
It only needed to be done once. Now send this code
to GitHub.

```sh
git push -u origin master
```

That command *will* be repeated many times during this session.

## Create the file package.json

Create the following package.json but modify it for your
own use (directions follow):

```json
{
  "name": "vuepress-netlify-github",
  "version": "1.0.0",
  "description": "Configuring VuePress to work with Netlify and GitHub",
  "main": "index.js",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/tomcam/vuepress-netlify-github.git"
  },
  "author": "Tom Campbell <tomcampbell@gmail.com>",
  "license": "MIT",
  "private": true,
  "scripts": {                                                                  
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  },
  "homepage": "https://github.com/tomcam/vuepress-netlify-github#readme",
  "dependencies": {
    "vuepress": "^0.14.0"
  }
}
```

* Replace `vuepress-netlify-github` with the name of your repo.
* Replace `Configuring VuePress to work with Netlify and GitHub` with
a description that matches what your repo does
* Replace `vuepress-netlify-github.git` with the name of your repo followed by `.git`
* Replace the author information with your own
* Replace the `tomcam` and `vuepress-netlify-github` with your
username and repo name, respectively: `https://github.com/tomcam/vuepress-netlify-github#readme `

Now commit it to source code:

```sh
git add package.json
git commit -m "Add deployment to docs directory"
git push -u origin master
```

## Let's see it working!

Time to run VuePress:

```sh
yarn docs:dev
```

Browse to `http://localhost:8080` and take a look at your masterpiece.

## Directory paths work as expected

The whole point to this configuration is to store everything
in `/docs/` instead of the root directory. Let's see how this 
works in a concrete way.

* Create a new directory called `public` in the `.vuepress` directory:

```sh
mkdir -p ~/Dropbox/work/vuepress-netlify-github/docs/.vuepress/public
```

* Add an image file to the `.vuepress/public` directory
you just created. You can use this example, which is from
[Pixabay](https://pixabay.com/illustrations/website-responsive-creative-design-3374825/) and is freely usable.

* Insert the following code at the top of README.md:

```md
![Generic biz pic](/wordpress-site-1280.jpg)
```

Note that `/docs/` does not appear anywhere. VuePress rewrites
the URLs so that `/docs/.vuepress/public` looks like it's
in root.

## Adding your repo to Netlify

Netlify hosts static sites for free, and it has full support
for VuePress! Here's how to get your site out to the
world for the low low price of absolutely nothing.

I'm assuming here you've [signed up for Netlify](https://app.netlify.com/signup),
which simply requires your GitHub account. Slick.

* Go to [Netlify](https://netlify.com) and choose
[New site from Git](https://app.netlify.com/start)

* Under **Continuous Deployment** choose GitHub.

Depending on how you've configured Netlify you're given a list
of all your repos, or you'll have to type in the appropriate one.
Select it however you need, 

The provided deploy settings work just fine:

**Build command** `vuepress build docs`

**Publish directory** `docs/.vuepress/dist`

* Choose **Deploy site** and watch the magic happen!


## Credits

Image by [kreatikar](https://pixabay.com/users/kreatikar-8562930/) from [Pixabay](https://pixabay.com/illustrations/website-responsive-creative-design-3374825/)
