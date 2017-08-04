---
layout: post
title:  "免费私有远程git仓库=dropbox+git"
date:   2012-07-11 20:11:17 +0800
categories: git
---

## Step 1:

```bash
~/project $ git init
~/project $ git add .
~/project $ git commit -m "first commit"
~/project $ cd ~/Dropbox/git
```

## Step 2:

```bash
~/Dropbox/git $ git init --bare project.git
~/Dropbox/git $ cd ~/project
```

## Step 3:

```bash
~/project $ git remote add origin ~/Dropbox/git/project.git
~/project $ git push origin master
```