<p align="center">
  <a href="https://github.com/marketplace/actions/github-followers">
    <img alt="GitHub Followers Logo" width="200px" src="https://github.com/JieDing/github-followers-action/raw/main/assets/followers.png">
  </a>
</p>
<h1 align="center">
  GitHub Followers Display Action 👥
</h1>
<p align="center">

  <a href="https://github.com/JieDing/github-followers-action/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/License-Apache_2.0-green.svg" alt="License">
  </a>  

  <a href="https://golang.org/">
    <img src="https://img.shields.io/badge/Language-Go-blue.svg" alt="Language">
  </a>

  <a href="https://codecov.io/gh/JieDing/github-followers-action/branch/main">
    <img src="https://codecov.io/gh/JieDing/github-followers-action/branch/main/graph/badge.svg" alt="Code coverage status badge">
  </a>
<!--
  <a href="https://github.com/JamesIves/github-pages-deploy-action/releases">
    <img src="https://img.shields.io/github/v/release/JamesIves/github-pages-deploy-action.svg?logo=github" alt="Release version badge">
  </a>

  <a href="https://github.com/marketplace/actions/deploy-to-github-pages">
    <img src="https://img.shields.io/badge/action-marketplace-blue.svg?logo=github&color=orange" alt="Github marketplace badge">
  </a>
-->
</p>

<p align="center">
  Do you want to show your followers in your GitHub profile? Do you want to know which of your followers are more active or have a better influence?
This github-followers-action ranks your followers according to different criteria(number of their followers, credits of their repos, number of their contributions and number of people they're following). 
It also renders those data into HTML elements so that those ranked followers can be easily displayed in your GitHub profile.
This action is inspired by <a href="https://github.com/ouuan/ouuan">ouuan's Profile</a>.
</p>

## Getting Started 🚀

### Set up Your Profile Repo

Create a Repository, and name it as your username. See my example [here][JieDing].

`Username/Username` is a special repository since its README.md will appear on your public profile.

### Set Start and End Flags

Once created your profile, you can add whatever you want into the README.md.

The only thing you need to do is to add following flags to your README.md, so that this action can know where your ranked followers should be placed at.

The start and end flag:
```html
<!--ACTION_START_FLAG:github-followers-->
<!--ACTION_END_FLAG:github-followers-->
```

An example of README.md (before adding flags):
```html
Hi there 👋

This is JieDing. 

Here are some fun facts: ......
```

After adding two flags:
```html
Hi there 👋

This is JieDing. 

Here are some fun facts: ......

<!--ACTION_START_FLAG:github-followers-->
<!--ACTION_END_FLAG:github-followers-->
```

### Build Your Workflow

Create the directory `.github/workflows` in the `Username/Username` repository.

Add the following example to `.github/workflows` directory.

`github-followers.yml`
```yaml
on:
  push:
    branches:
      - main
  schedule:
    - cron: '0 20 * * *'
jobs:
  github_followers_job:
    runs-on: ubuntu-latest
    name: A job to display github followers in your profile
    steps:
      - uses: actions/checkout@v3

      - name: use github-follower
        id: github-follower
        uses: JieDing/github-followers@main
        env:
          login: ${{ github.repository_owner }}
          pat: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      - name: Commit changes
        run: |
            git config --local user.email "action@github.com"
            git config --local user.name "GitHub Action"
            git add -A
            git diff-index --quiet HEAD || git commit -m "Update GitHub followers"
      - name: Pull changes
        run: git pull -r
      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: ${{ github.ref }}
```

### Configuration



[JieDing]: https://github.com/JieDing/JieDing