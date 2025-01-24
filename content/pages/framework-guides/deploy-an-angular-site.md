---
pcx_content_type: how-to
title: Angular
---

# Angular

[Angular](https://angular.io/) is an incredibly popular framework for building reactive and powerful front-end applications.

In this guide, you will create a new Angular application and deploy it using Cloudflare Pages.

## Create a new project using the `create-cloudflare` CLI (C3)

Use the [`create-cloudflare`](https://www.npmjs.com/package/create-cloudflare) CLI (C3) to set up a new project. C3 will create a new project directory, initiate Angular's official setup tool, and provide the option to deploy instantly.

To use `create-cloudflare` to create a new Angular project, run the following command:

```sh
$ npm create cloudflare@latest my-angular-app -- --framework=angular
```

`create-cloudflare` will install dependencies, including the [Wrangler](/workers/wrangler/install-and-update/#check-your-wrangler-version) CLI and the Cloudflare Pages adapter, and ask you setup questions.

{{<Aside type="note" header="Git integration">}}

The initial deployment created via C3 is referred to as a [Direct Upload](/pages/get-started/direct-upload/). To set up a deployment via the Pages Git integration, refer to the [Git Integration](#git-integration) section below.

{{</Aside>}}


{{<render file="/_framework-guides/_git-integration.md">}}

### Create a GitHub repository

{{<render file="/_framework-guides/_create-gh-repo.md">}}
<br/>

```sh
# Skip the following three commands if you have built your application
# using C3 or already committed your changes
$ git init
$ git add .
$ git commit -m "Initial commit"

$ git branch -M main
$ git remote add origin https://github.com/<YOUR_GH_USERNAME>/<REPOSITORY_NAME>
$ git push -u origin main
```

### Create a Pages project

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/) and select your account.
2. Go to **Workers & Pages** > **Create application** > **Pages** > **Connect to Git** and create a new Pages project.

You will be asked to authorize access to your GitHub account if you have not already done so. Cloudflare needs this so that it can monitor and deploy your projects from the source. You may narrow access to specific repositories if you prefer; however, you will have to manually update this list [within your GitHub settings](https://github.com/settings/installations) when you want to add more repositories to Cloudflare Pages.

3. Select the new GitHub repository that you created and, in the **Set up builds and deployments** section, provide the following information:

{{<pages-build-preset framework="angular">}}

Optionally, you can customize the **Project name** field. It defaults to the GitHub repository's name, but it does not need to match. The **Project name** value is assigned as your `*.pages.dev` subdomain.

4. After completing configuration, select the **Save and Deploy**.

Review your first deploy pipeline in progress. Pages installs all dependencies and builds the project as specified. Cloudflare Pages will automatically rebuild your project and deploy it on every new pushed commit.

Additionally, you will have access to [preview deployments](/pages/configuration/preview-deployments/), which repeat the build-and-deploy process for pull requests. With these, you can preview changes to your project with a real URL before deploying your changes to production.

{{<render file="/_framework-guides/_learn-more.md" withParameters="Angular">}}