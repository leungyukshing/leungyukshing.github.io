---
title: Cookiecutter
mathjax: true
date: 2023-06-30 23:38:23

---

# Cookiecutter

Cookiecutter is a command-line tool used to create structured projects from a template. It's a widely-used tool in the industry and it also support engineers to customize their own templates and share with others. In this post, I would like to.share some basic information about cookiecutter.

<!-- more -->

# Introduction

Consider a scenario that your organization maintains a brunch of repositories. And everytime when you try to add one, you want it generated automatically with a basic template, like containing a README, LICENSE. Moreover, you may also want to generate makefiles automatically to build a project. All of these requirements makes cookiecutter a good choice for us. We can define the project template and then use it later for more than one time.

# How to use it?

First you need to install the command line ([Installation Guid](https://cookiecutter.readthedocs.io/en/1.7.2/installation.html)). There are different ways to install on different platforms.

After installation, you can use it. But you need a template. You can use either a public template (Usually on Github) or a local one. I created a simple cookiecutter template to generate a project that contains a python file.

```bash
$ cookiecutter https://github.com/leungyukshing/cookiecutter-simple.git
```

Then you can see the command line will prompt you to enter some information about the project with provided default values.

# Make your own template

Using cookiecutter is very easy and I think making our own template is more important. Bascally we use ``{{cookiecutter.[name]}}`` to define the parameters. Then we provide a `cookiecutter.json` to provide default values for each parameters. After generation, all the `{{cookiecutter.[name]}}` will be replaced by the actual value of `[name]`, which could be a default one or from user's input.