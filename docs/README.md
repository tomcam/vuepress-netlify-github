![Generic biz pic](/wordpress-site-1280.jpg)
# Configuring VuePress to work with Netlify and GitHub

By Tom Campbell, creator of [VuePress Book](http://vuepressbook.com).
Find this code on [GitHub](http://github.com/tomcam/vuepress-netlify-github).

[VuePress](https://v1.vuepress.vuejs.org/) likes its
text to be in a `/doc/` directory off root. If you 
play your cards right it will rewrite the `/doc/` part
out of URL. This article shows you how to set up
your workflow so VuePress, Netlify, and GitHub all
know what to do in terms of transforming your 
[Markdown](https://github.github.com/gfm/)
files into a cohesive website that previews will on your
machine and upon deployment to the real world by Netlify.

## Creating a GitHub repo: WATCH CAREFULLY!

Now do exactly as I say here unless you're a 
Git expert.

### Assumptions

* I'm assuming your code will live in a directory 
located at `~/Dropbox/work`, but obviously 
it could be anywhere.


* In this example the repo name is `vuepress-netlify-github`.
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
mkdir -p ~/Dropbox/work/presshosting/docs/.vuepress
```

## Creating your root README.md file

* Change to the root directory.

```sh
cd ~/Dropbox/work/presshosting
```

* Create a file called README.md in the `/docs/` directory.

```sh
nvim ~/Dropbox/work/presshosting/docs/README.md
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

```sh
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
    "url": "git+https://github.com/tomcam/presshosting.git"
  },
  "author": "Tom Campbell <tomcampbell@gmail.com>",
  "license": "MIT",
  "private": true,
  "scripts": {                                                                  
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  },
  "homepage": "https://github.com/tomcam/presshosting#readme",
  "dependencies": {
    "vuepress": "^0.14.0"
  }
}
```

* Replace `vuepress-netlify-github` with the name of your repo.
* Replace `Configuring VuePress to work with Netlify and GitHub` with
a description that matches what your repo does
* Replace `presshosting.git` with the name of your repo followed by `.git`
* Replace the author information with your own
* Replace the `tomcam` and `presshosting` with your
username and repo name, respectively: `https://github.com/tomcam/presshosting#readme `

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
mkdir -p ~/Dropbox/work/presshosting/docs/.vuepress/public
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

## Credits

Image by [kreatikar](https://pixabay.com/users/kreatikar-8562930/) from [Pixabay](https://pixabay.com/illustrations/website-responsive-creative-design-3374825/)
