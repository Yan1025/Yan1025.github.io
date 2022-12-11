---
title: "How to build this blog?"
date: 2022-12-11T21:57:11+08:00
draft: false
---
## Tech stack
* [GitHub Pages](https://pages.github.com/)
* [Hugo](https://gohugo.io/getting-started/quick-start/)
## Notice
There is a conflict on the way to use [Git Actions](https://github.com/features/actions).

[Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/) on Hugo's official website indicates that
we should create branch named 'gh-pages' as the source branch bearing the complied static website's codes by 'hugo' 
command, and create a file in .github/workflows/gh-pages.yml containing the following content :
```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch that will trigger a deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
For comparision, the **Hugo Action** recommended by [GitHub](https://github.com/Yan1025/Yan1025.github.io/actions/new) 

[//]: # (![Hugo Action]&#40;https://raw.githubusercontent.com/Yan1025/picbed/master/picbed/img.png&#41;)
![](https://github.com/Yan1025/picbed/blob/master/picbed/img.png?raw=true)
does not needs creating extra branck besides 'main' branch, therefore it's more convennient than above.

In addition, GitHub will automatically create a file in .github/workflows/hugo.yml containing the following content:
```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.102.3
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v2
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```
Those two solutions are both effective by my own test.

Personally, i choose the latter because that's more concise and simple.

