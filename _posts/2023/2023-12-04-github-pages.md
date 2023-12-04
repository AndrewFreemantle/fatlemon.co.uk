---
title: 'How to build and deploy GitHub Pages with custom gems'
permalink: /2023/12/custom-gems-with-github-pages/
excerpt: "How to fix the GitHub Pages build action warning: \"github-pages can't satisfy your Gemfile's dependencies\" when deploying a site with custom gems"
tags:
  - Github Pages
  - Gems
  - Ruby
  - Gemfile
---

GitHub's own gem for `github-pages` [includes a comprehensive list of dependencies](https://pages.github.com/versions/) that GitHub uses to build and deploy our static site. However, if we add any extra Ruby Gems to our `Gemfile`, the default GitHub build action <em>succeeds</em> but we're shown and emailed the following warning:

![Warning message from GitHub Pages build stage]({{ site.imageurl }}2023/github-actions/github-action-build-warning.png)
<figcaption>{Pages Repository} → Actions → pages build and deployment → {most recent run}</figcaption>

Our static site is also missing the features of our extra gems 😞

Fortunately, it's a straightforward fix of 2 steps:

## 1. Add a new build and deploy Action
![Annotated screenshot of the steps to add a new workflow]({{ site.imageurl }}2023/github-actions/add-workflow.png)

1. With the site repository open, select `Actions` and then <kbd>New workflow</kbd>
1. Search for 'jekyll'
1. Find the <strong>Jekyll</strong> workflow and click <kbd>Configure</kbd>

This will open an editor. The only thing we need to check and possibly change is around line 39:

> `ruby-version: 3.1` # Not needed with a .ruby-version file

Jekyll doesn't currently support v3, so if we have a `.ruby-version` file in our repo we can delete or comment out this line, or if we don't, we can simply change this to:

> `ruby-version: 2.7.4`

<p class="notice--info text--info">
  <i class="fas fa-fw fa-note-sticky"></i> The current version of Ruby that GitHub Pages supports for build environments is <a href="https://pages.github.com/versions/">documented</a> and at the time of publication is `2.7.4`
</p>

<ol start="4">
  <li>Click <kbd>Commit changes...</kbd> and fill in the commit message.</li>
</ol>

Now to tell GitHub to use it...

## 2. Configure GitHub to use our new Workflow
![Annotated screenshot of the steps configure GitHub to use the new workflow]({{ site.imageurl }}2023/github-actions/configure-build.png)

Now we just need to tell GitHub to use our new workflow:

1. Select `Settings`
1. Then, from the left-hand menu choose `Pages`
1. Find the `Build and deployment` heading, and change the `Source` from the default `Deploy from a branch` to `GitHub Actions`

That's it! <i class="fa-brands fa-github-alt"></i>
