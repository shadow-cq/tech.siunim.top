---
title: How to fix the Gatsby command not found issue
abbrlink: 82dde210
date: 2020-12-13 11:21:18
tags:
---




# How to fix the Gatsby command not found issue

ref: [https://reactgo.com/gatsby-command-not-found/](https://reactgo.com/gatsby-command-not-found/)

In this tutorial, we are going to learn about how to fix the `Gatsby: command not found` issue after installing the `gatsby-cli`.



When we install a `gatsby-cli` using the `npm i -g gatsby-cli` command and trying to use a `gatsby` command sometimes we will see the following issue in our terminal.



```bash
-bash: gatsby: command not found
```

This issue occurs due to the mismatch of the npm config prefix option.

To fix this issue follow the below steps:

1. First, delete the npm path prefix by running the following command.

```bash
npm config delete prefix
```

1. Set a path prefix to `/usr/local`.

```bash
npm config set /usr/local
```

1. Now, install the gatsby-cli globally by using the following command.

```bash
npm install -g gatsby-cli
```

> Note: If you are using mac then you need to add `sudo` before the above command.

1. Verify the successful installation by running the `gatsby -v` command.

```bash
gatsby -v
```

If it outputs the version number like this, then you are successfully installed.

```bash
Gatsby CLI version: 2.12.51
```

You can now create new gatsby projects like this.

```bash
gatsby new my-new-gatsby-blog
```