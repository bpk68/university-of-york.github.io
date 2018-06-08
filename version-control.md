---
layout: page
title: Version Control
published: true
---

This page outlines our current versioning control methods and conventions. It's important that we agree as a team on how we approach and use these conventions and most importantly, improve them as we go. 

By using the agreed branching and versioning habits below, we will create a clearer picture of our development progress, release versions and code history. We will also make the release process and branch merging simpler.

## Version control key points

-We use Git for our version control
-Our code repositories are stored on GitHub
-We use a range of access tools and software as per team preference. For example, many prefer to use the command line, whilst others use GUI software such as [GitHub's Desktop program](https://desktop.github.com/) or [GitKraken](https://www.gitkraken.com/).

## How we structure our Git branching

Take a look at the following diagram of a well documented and implemented Git branching approach:

![git.png]({{site.baseurl}}/git.png)

This illustrates how we can keep our current master branch containing our versioning history, but introduces a finite set of other, dedicated branches to suit future development.

## How this works in practice

The left to right lines indicates time and you can see the relationship between branches with the arrows, but there are some things to point out.

### Master branch

The master branch is the main, historical record-keeping trunk for our code base. It stores the official release history that is created and built up from the other branches. 

It would contain commits such as v1.5.6, v2.0, v2.3.1, etc., both major and minor releases.

With the exception of hotfixes, **nothing other than release branches are merged into Master.**

### Development branch

The development branch is similar to the Master, in that it is a _trunk_, but it is the default, active working code base. The development branch contains an ongoing body of work to be released in the next available release version.

It is branched off into features for larger bodies of work or as sprint tasks or other complex changes dictate.

The vast majority of development work is carried out against the Development branch. The only exceptions to this are:

-Minor tweaks done in a Release branch when a release is imminent (see below section on releases)
-Hotfixes needed on the Master branch (see below section on hotfixes)

### Feature branches

Each new feature (a smaller, but complete piece of work) should reside in its own branch which can then be pushed into the central repository (i.e. pushed back into the Development branch) for backup/collaboration.

Feature branches are cloned from the Development branch. When work on the feature is complete and ready to be merged, a pull-request should be initiated to allow a code review to take place (see [Code Review](/code-reviews) page).

Once code reviewed and approved, the branch is merged back into the Development branch. 

**Features should never directly interact with the Master branch!**

#### Following a Feature branch merge

Once a Feature branch has been merged into the Development branch (i.e. it has generally been tested and accepted as done), it should now be **deleted from the repository**. 

### Release branch

When a release is due (either there is sufficient work done, features created and completed, or the time is nigh) a Release branch is created from the Development branch.

This signals the start of the next release cycle. Once a release cycle has started, it changes the development process a little in the following ways:

1.**No new features can be added** to the Release branch
2.**Only minor tweaks, bug fixes and documentation items** should be carried out in and committed to the Release branch

#### The release process

Once the code is ready to release, the following things should happen:

1.The release is carried out, the code pushed to the live base
2.The Release branch is **merged back into the Master branch** and **tagged with a version number**
3.The Release branch is **merged back into the Development branch**
4.The **Release branch is now deleted.**

### Hotfix branch

Otherwise known as ‘maintenance’ branches, Hotfix branches are much like the Release and Feature branches except that they fork directly from the Master branch.

For interim, serious or emergency fixes, a Hotfix branch should be created. Once the fix is complete, the following actions should happen:

1.The Hotfix branch should be **merged into the Master branch**
2.The Master branch should be **tagged with an updated version number**
3.The Hotfix branch should be merged into the Development branch _or_ the current Release branch, depending on how close to a release the team are
4.The **Hotfix branch should be deleted** 

## Branch naming conventions

Beyond the standard, evergreen branches of Master and Development, for new branches, fixes, releases, etc. the following naming conventions should be used:

- [Feature] > feature/name-of-feature
- [Hotfix] > fix/name-of-fix
- [Release] > release/1.0.0

Here are some examples:

- [Feature] > feature/responsive-search-layout
- [Feature] > feature/history-landing-page
- [Feature] > feature/listing-components
- [Release] > release/1.5.5
- [Release] > release/1.7
- [Hotfix] > fix/administrate-browser-issue
- [Hotfix] > fix/carousel-display-bug

## A word on experimental work

Any good development team knows that experimentation and tangential coding can produce new ideas and help improve the overall code base. However, this code usually lives outside of normal work, such as sprints or other planned features and could potentially introduce more errors or issues.

For these experimental or playground bodies of work, we recommend creating a separate branch and continuing to work on these features locally. 

The experimental branch naming convention is as follows:

-[Experimental] > exp/name-of-experimental-work


### Note 1 - Experimental collaboration

By using the branching system above, experimental work can still be collaborated on by the team, so long as the branch is published to the repository.

### Note 2 - Keeping up to date with Development branch

The closer to the main Development trunk/branch the experimental branch is, the easier it will be to merge back in. Of course, this isn't always possible, especially if your branch is wildly different or quite radical in nature. 

However, where possible the code should be kept up to date with any changes in the Development branch by maintaining regular refreshes.

### Note 3 - Merging experimental branches

Experimental branches should be merged in the same way as regular features. First, make sure the experimental code is tested and working. Next, initiate a pull request to make sure that your code is reviewed and accepted/approved by the team. Once approved, the pull request will be merged back into the Development branch at the next available point.

Again, once the branch is successfully merged it should be deleted from the repository.



