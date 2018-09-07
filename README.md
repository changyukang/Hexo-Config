# Hexo-Config
* Hexo configuration for github blog;
* Theme is managed by git submodule.

1. gitclone Hexo-Config in a new computer, need to do:
  * install node & npm
  * install hexo
  * gitclone Hexo-Config in a local directory
  * run "npm install" in the directory, this will create "npm-modules" in this directory. This directory is in .gitignore, so it will not be downloaded from github.
  * run "git submodule init & git submodule" update in the directory, this will update the themes. The themes have been managed by submodule.
  * run "hexo g & hexo s", and then open the url in a browser, you can see the blog.
  * by doing things above, the "hexo init" do not to run in the directory.

2. problems solving:
  * WARN No layout: index.html
    Since the themes are managed by submodule, so need to update them firstly
  * Hexo g error
    Since node_modules are not managed in github, so need to run "npm install" in the directory
  * How to upgrade npm packages
    npm install -g npm-check
    npm-check

    npm install -g npm-upgrade
    npm-upgrade

    ps: not all the packages can be upgrade by runing commands above, so you may need to access the package website and run commands given officially

  * package-lock.json dependency security vulnerabilities
    This can be solved by upgrade these packages



