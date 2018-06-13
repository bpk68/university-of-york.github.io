---
layout: page
title: Release Process
published: true
---

At some point, development work pauses and shifts towards releasing the latest, stable bundle of code for a particular project. This is our release process and consists of a number of steps to ensure that:

-All current, active development work is bundled into a single release branch
-The release branch is tested, is error-free and conforms to our [style guides](https://university-of-york.github.io/style-guide/)
-The release code is successfully deployed
-Versions are updated
-Trunks (i.e. Master and Development branches) are merged into and updated


## The release process

The following sections offer a geneneral outline of how to get ready for and perform a release of the working code base.

In essence, the process looks like this:

1. Make sure all code involved in the release is checked in, tested and ready to release
2. Create a Release branch following our [Git branching and naming conventions](https://university-of-york.github.io/version-control/) and give it the next version number (major, minor, patch) and update the version numbers
3. Trigger deployment through continuous integration
4. Test deployment is successful
5. Merge Release branch into Master and Development branches
6. Delete Release branch

### Collecting new work

Each release should have a designated release lead, a single point of responsibility that ensures that this release happens in the correct sequence and has everything they need for a successful one.

Firstly, the release lead should notify key stakeholders of the upcoming release and make sure that:

-Development work is current and up to date
-Development work is committed and checked into the Development branch of the project
-The wider digital team are satisfied that the code can be released - i.e. it passes quality assurance

**Note** as code is being checked in, it should be subject to the [code review process](https://university-of-york.github.io/code-reviews/) by both peers and automation. It _should not_ be merged back into the Development branch unless it has passed code review!

### Create a release branch

Create a new branch off the Development branch, following our naming conventions with the latest appropriate version number. 

For example, if the due release is a minor release, then the branch you create will be `release/v1.5.7`. 

Be sure to update the versioning information in the appropriate file(s). This might include:

-Readme.md files
-Package.json or Webpack files
-Other configuration files

This may change over time as our integration with more automated review and deployment systems grows.

### Trigger deployment

**In development**

Our ultimate aim is to make use of a continuous integration (CI) platform, such as TeamCity or Travis CI to build, publish and deploy our approved code.

The process is still in development, but the idea would be that once a release branch is complete, a build and deployment would be triggered by the CI platform to automate what can be a very error-prone process.

Currently, our deployment is a manual effort that involves opening an FTP connection to the relevant area on the web server and transferring the files over.


### Test deployment success

Once a deployment has finished, it's always a good idea (**read** imperative) to go and have a look at the shiny new code and make sure that the application or website is working correctly and that the deploy has been successful.

### Post-release Git tasks

Once the release has been successful, it's time to carry out a couple of additional tasks in our [version control](https://university-of-york.github.io/version-control/) system.

#### 1 - Merge the Release back into the trunk

The Release branch contains the latest, most up-to-date version of the code base at this point in time. In order to keep the code versioning history accurate as well as sharing the latest code with the ongoing development efforts, the Release branch should be merged back into the following two branches: 

-**The Master branch** - this allows the current version number that was released to act as a marker on the code base timeline.
-**The Development branch** - this allows the development code base to receive the most up to date, stable, released code 

#### 2 - Delete the Release branch

Once a release is finished with and the branch has been merged back, the Release branch should be deleted.