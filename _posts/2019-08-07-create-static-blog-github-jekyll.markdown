---
layout: post
title:  "Create Static Blog Using Github and Jekyll"
date:   2019-08-07 21:06:35 -0400
categories: 
---
According to [GitHub Pages][github-pages] the name of the new repository should use the *username.github.io* format.

```
sudo apt install ruby gem git
sudo gem install jekyll
jekyll new sitename
bundle exec jekyll serve
```

You should now be able to access the blog locally at localhost:4000

To publish:

```
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/username.github.io
git push origin master

```

[github-pages]: https://pages.github.com/

