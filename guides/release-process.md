---
layout: page
title: Release Process
published: true
---

At some point, development work pauses and shifts towards releasing the latest, stable bundle of code for a particular project. This is our release process and consists of a number of steps to ensure that:

- All current, active development work is bundled into a single release branch
- The release branch is tested, is error-free and conforms to our [style guides](https://university-of-york.github.io/style-guide/)
- The release code is successfully deployed
- Versions are updated
- Trunks (i.e. Master and Development branches) are merged into and updated

## Setting up automated release and versioning

You can read more about how we automate versioning and release tagging in our [automated versioning](https://university-of-york.github.io/guides/automated-versioning/) documentation.


## The release process

The following sections offer a geneneral outline of how to get ready for and perform a release of the working code base.

Firstly, when we're nearing a release, we should do the following:

1. Make sure all code involved in the release is checked in, tested and ready to release
2. Create a Release branch following our [Git branching and naming conventions](https://university-of-york.github.io/version-control/) and give it either a suitable name or release number according to our last GitHub release
3. Create a GitHub Pull Request (PR) for the release branch to be merged back into `master`
4. Further work, small amends, bug fixes, etc. should all be carried out in this latest release branch.

Once the release is finalised and ready to be physically released, in essence, the process looks like this:

1. Once happy, complete the PR and merge this `release` branch back into `master`
2. Semaphore CI will start to build the `master` branch and run the extra, final set in the build process `yarn release`
3. Our automated versioning and release system [Semantic Release](https://github.com/semantic-release/semantic-release) will process the bundles commits and automatically increment the version release number as appropriate, pushing this back to GitHub
4. Trigger a manual deployment on Semaphore to push the working code live
5. Test deployment is successful
6. Post deploy/release tasks
	- Merge `master` back into the `dev` branch

Breaking down the various steps a little further...

### Collecting new work

Each release should have a designated release lead, a single point of responsibility that ensures that this release happens in the correct sequence and has everything they need for a successful one.

Firstly, the release lead should notify key stakeholders of the upcoming release and make sure that:

- Development work is current and up to date
- Development work is committed and checked into the `dev` branch of the project
- The wider digital team are satisfied that the code can be released - i.e. it passes quality assurance (QA) and testing

<div class="c-alert c-alert--info" role="alert">
  <div class="c-alert__content">
    <strong>Note:</strong> as code is being checked in, it should be subject to the <a href="https://university-of-york.github.io/code-reviews/">code review process</a> by both peers and automation. It <em>should not</em> be merged back into the <code>dev</code> branch unless it has passed code review!
  </div>
</div>


### Create a release branch

Create a new branch off the `dev` branch, following our naming conventions with the latest appropriate version number or a suitable name. 

For example, if the due release is a minor release, then the branch you create will be `release/v1.5.7`. Or you could create a release name from a collective body of work title, such as `release/summer-map-updates` or even a sprint, such as `release/sprint-5`

<div class="c-alert c-alert--info" role="alert">
  <div class="c-alert__content">
    <strong>Note:</strong> that the final release version will be determined and published by our automated release numbering system <a href="https://github.com/semantic-release/semantic-release">Semantic Release</a>.
  </div>
</div>


### Trigger deployment

We use a platform called Semaphore CI to manage our automated build and deployment processes. Essentially, when you check in a branch, Semaphore picks up the changes and triggers a build. For development work, successful builds are automatically deployed to the correct preview location for testing and QA.

For releases commited to the `master` branch, these must be manually deployed to reduce the potential for error on our live, highly-trafficked website. The process is still automated from a deployment point of view, but the deployment must be started by a simple button click on the Semaphore dashboard.

For more information on our automated continuous integration and deployment, you can read our [build and deployment](https://university-of-york.github.io/guides/build-deployment/) documentation.


### Test deployment success

Once a deployment has finished, it's always a good idea (**read** imperative) to go and have a look at the shiny new code and make sure that the application or website is working correctly and that the deploy has been successful.

### Post-release Git tasks

Once the release has been successful, it's time to carry out a couple of additional tasks in our [version control](https://university-of-york.github.io/version-control/) system.

#### 1 - Merge the Release back into the trunk

Following the Pull Request merge into the `master` branch, this body of code now contains the latest, most up-to-date version of the code base at this point in time. In order to keep the code versioning history accurate as well as sharing the latest code with the ongoing development efforts, the released `master` branch should be merged back into the following branch:

- **The Development branch** - this allows the development code base to receive the most up to date, stable, released code 

#### 2 - Delete the Release branch

Once a release is finished with and the branch has been merged back, the `release` branch should be deleted.

<div class="c-alert c-alert--info" role="alert">
  <div class="c-alert__content">
    <strong>Note:</strong> that this can occur once a successful merge has taken place following a release Pull Request.
  </div>
</div>
