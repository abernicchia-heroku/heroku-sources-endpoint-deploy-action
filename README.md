# Heroku Sources endpoint deploy GitHub Action
GitHub Action to deploy a Heroku from a private repository on push requests using the [Heroku Source Endpoint API](https://devcenter.heroku.com/articles/build-and-release-using-the-api#sources-endpoint) to upload the code. 
In a GitHub Workflow, this action requires to be preceeded by the [actions/checkout](https://github.com/actions/checkout) to work properly.

## Disclaimer
The author of this article makes any warranties about the completeness, reliability and accuracy of this information. **Any action you take upon the information of this website is strictly at your own risk**, and the author will not be liable for any losses and damages in connection with the use of the website and the information provided. **None of the items included in this repository form a part of the Heroku Services.**

## How to use it
Create the following GitHub workflow under `.github/workflows` directory within your repository and add the following YAML file and configure it. If you need to filter files from your repository before deploy use the `sparse-checkout` option available with `actions/checkout`.

This will be executed on push events
```
# push.yml
name: Deploy push

on:
  push:
    paths-ignore:
      - '.github/workflows/**'
    branches:
      - main

jobs:
  build-push:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v4
        with:
          sparse-checkout: |
            /*
            !.gitignore
            !.github
      - uses: abernicchia-heroku/heroku-sources-endpoint-deploy-action@v1
        with:
          heroku-api-key: ${{secrets.HEROKU_API_KEY}} # set it on GitHub as secret
          heroku-app-name: ${{vars.HEROKU_APP_NAME}} # set it on GitHub as variable at repository level
          remove-git-folder: false # if you want to override the default (true) - it's usually recommended to avoid exposing the .git folder 
```