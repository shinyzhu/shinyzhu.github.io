---
title: "GitHub Action Not Running Case Fixed"
description: "Be aware of your secrets or variables."
date: 2024-02-19T14:20:00+08:00
lastmod: 2024-02-19
tags: [github, devops, cicd, devtools]
categories: Blog
featured_image: "/posts/2024/github-action-not-running-case/github-action-not-running-fixed.png"
images: ["/posts/2024/github-action-not-running-case/opengraph-card.png"]
---

## What Happened to My GitHub Action

I wanted to add a new html file to the `www` of [this project](https://github.com/shinyzhu/aibeixin_web). The workflow should be working correctly. But it didn't run when I pushed to the repo.

It didn't show any error to me either.

Furthermore, I didn't find the "Run workflow" button under the Action tab.

Here is the original [workflow file](https://github.com/shinyzhu/aibeixin_web/blob/main/.github/workflows/cd.yml) `cd.yaml`:

```yaml
name: Ship It!
on:
  push:
    branches:
      - main
    paths:
      - www/*
jobs:
  shipping:
    name: ship to production
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4

      - name: Install SSH Key
        uses: shimataro/ssh-key-action@v2
        with:
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          known_hosts: never_know

      - name: Add Known Hosts
        run: ssh-keyscan -p ${{ secrets.SSH_PORT }} -H ${{ secrets.SSH_HOST }} >> ~/.ssh/known_hosts
            
      - name: Deploy with rsync
        run: rsync -avz --delete -e "ssh -p ${{ secrets.SSH_PORT }}" ./www/ ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }}:{{ secrets.PUB_PATH }}
```

Probably you've found **what's wrong** there.

But I'd show what I've done for fixing it.

### 1, Check new versions of Actions

I edited the `cd.yaml` in the web page. So I can find new versions of actions I used on the right panel. They are just under the "Marketplace" tab.

Nothing happened.

### 2, Make the "Run workflow" Button Available

I remember that you can click a button to manually run a workflow. But I'm confused for a while because I can't do that now.

And I [learned this](https://stackoverflow.com/a/72472817/21465) to show the button:

```yaml
name: Ship It!
on:
  workflow_dispatch: {}
  push:
    branches:
      - main
```

I added a simple line of code `workflow_dispatch: {}` into the `cd.yaml` just under `on:`. Then I can see the "Run workflow" button.

I clicked the button.

And. It showed error at least.

```sh
Run rsync -avz --delete -e "ssh -p ***" ./www/ ***@***:{{ secrets.PUB_PATH }}
  rsync -avz --delete -e "ssh -p ***" ./www/ ***@***:{{ secrets.PUB_PATH }}
  shell: /usr/bin/bash -e {0}
Unexpected remote arg: ***@***:{{
rsync error: syntax or usage error (code 1) at main.c(1512) [sender=3.2.7]
Error: Process completed with exit code 1.
```

### 3, Check Action Secrets

I believe I've set the value of PUB_PATH correctly. So I checked [the docs](https://docs.github.com/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets):

```yaml
steps:
  - name: Hello world action
    with: # Set the secret as an input
      super_secret: ${{ secrets.SuperSecret }}
    env: # Or as an environment variable
      super_secret: ${{ secrets.SuperSecret }}
```

Ah! What I've missed is a '$' sign before using a secret.

## Case Fixed And Questions

by just adding a `$`. But here are some questions:

1. Why isn't there a syntax checking for using variables or secrets in a workflow?
2. Why isn't there a message for a GitHub Action not triggered?