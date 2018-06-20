---
layout: page
title: Build & Deployment
published: true
---

We use a few tools and platforms to help us manage and automate the error-prone, often manual task of building our code and deploying it. Otherwise known as **continuous integration (CI)** and **continuous deployment (CD)**, most of the work should be automated to a dedicated platform that can be triggered at various points.

For our code bases and development projects, we use the following:

- [Codacy](https://www.codacy.com/) for our [code reviews](https://university-of-york.github.io/code-reviews/)
- [Semaphore CI](https://semaphoreci.com) for our automated CI/CD processes

## Continuous integration

Semaphore CI is a simple, but powerful platform that automates the process of building and testing our code as per various project settings. The rough process looks like this:

1. A commit is published/pushed to the repository
2. Semaphore picks up this pushed change and triggers a build on that branch
3. The build either succeeds or fails
4. Post build actions,
	- Internal Slack channel 'digital_deployments' is updated with build status
	- (On success) for the `dev` branch, an automatic deployment to the preview location is triggered

**Note** for most of our repositories, new branches are automatically picked up and built within Semaphore, although it is _only_ the `dev` and `master` branches that are associated with deployment profiles. 

### Skipping a build

[Skipping a build, Semaphore CI docs](https://semaphoreci.com/docs/how-to-skip-building-for-some-commits-with-ci-skip.html)

If you'd like your commit, or a series of commits that you're pushing, to **not** trigger a build, just write `[ci skip]` or `[skip ci]` somewhere in your commit's message.

### Setting up a new project for CI

Most of the leg work for setting up a build project is done directly in [Semaphore CI](https://semaphoreci.com). 

You can read more on the [official documentation](https://semaphoreci.com/docs) but the general process is:

1. Log into Semaphore and click 'create new' and then 'project' from the top nav
2. Select the repository you wish to set up as a project
3. Select the branch you wish to set up the build on (_you can add more later_)
4. Set the owner of the project. This should be **university-of-york**, e.g. our organisation

#### General project settings

Once the project is set up, you'll need to add some project settings by clicking onto 'project settings', which is on the top right of your new project's dashboard.

1. Build Settings
	- Language > JavaScript
	- Node.js version > at least `node.js 8.11.1`
	- Setup > `yarn install`
	- Job #1 > `yarn build`
	- Job #1 > `yarn release` - **note** although we at this command here (because you can't set per-branch settings) this only actually does anything on the `master` branch because of the settings within a special file in the project
2. Environment Variables
	- Environment Variables from Secrets - make sure the `shared_ftp_details` secret file is loaded here as you'll need it for preview deployment via sftp (explained in next section). You can click 'manage secrets' to do this, or ask the owner/admin to add it to this project
	- Project specific Environment Variables - this is where you can set this repositories live deployment variables or project specific variables, such as ftp username and password.
3. Branches
	- Default branch > `master`
	- Cancellation strategy > Cancel queued builds except for the default branch
	- Fast failing > Fast failing enabled for all branches except the default one
	- Build new branches > Automatically, unless this project has a ton of new branches added then probably set it up as 'never'
4. Notifications
	- Email > this will be sent to your email address so set this as per your preference
	- Slack > Webhook URL > get this from the dev team leader or in the slack channel 'digital_deployments'
	- Slack > Builds > receive after: failed only, select branches: all
	- Slack > Deploys > receive after deploys: always, select required servers
	- Commit status > tick 'enable commit status'
5. Badge (optional)
	- Select the `master` branch and copy the 'Shields badge' markdown. Put this in the readme.md file in the project to pull in the build status badge on GitHub.

## Continuous deployment

After a successful build you'll want to make sure that the code is available for testing and review as part of our QA process. To do this, we make use of Semaphore's automated deployment servers. 

Generally, we have two deployment servers, a prevew for QA and testing purposes, and a live one that published final, tested, approved code. 

**Note** only the `dev` branch of a repository is deployed automatically and usually lives at the https://www.york.ac.uk/preview/[folder] url where '[folder]' is specific to the particular project in question. For example, the campus map deploys to '/preview/map'. 

The trunk branch, `master` is usually set up to be deployed, but it is left to be triggered manually. This avoids hasty or erroneous deployments. 

### Setting up a new deployment server

It's surprisingly straight forward to set up a deployment server via FTP using Semaphore. This is an example of setting up an automated deployment server for a typical `dev` branch.

1. Visit your project dashboard and click the '+' icon next to 'Servers'.
2. On the server options list, select 'Generic deployment'
3. Choose 'Automatic' - **note** for `master` branches, leave this as 'Manual'
4. For the deploy commands section, enter the following, each on a new line
	- `yarn build` - this will build the latest version and ensure things like the output folder is created before being deployed
	- `lftp -c "open -u $FTP_USER_PREVIEW,$FTP_PASSWORD_PREVIEW sftp://sftp.york.ac.uk; set ssl:verify-certificate yes; mirror -R ./dist ./[DEPLOY PATH]"`. Make sure you have `$FTP_USER_PREVIEW` and `$FTP_PASSWORD_PREVIEW` set up in the project's environment variables section and you set the appropriate path in place of '[DEPLOY PATH]'
5. Skip this SSH section
6. Give the server a meaningful name, something like 'Preview (project name)'.

You can edit the server settings whenever you wish by clicking on the server name from the dashboard and choosing 'Edit server' from the button that appears.


## Automatic release and versioning

We also employ an automated release and versioning process as part of successful builds on the `master` branch. This is covered more thoroughly in the [release process](https://university-of-york.github.io/guides/release-process/) and the [automated versioning](https://university-of-york.github.io/guides/automated-versioning/) documentation.
