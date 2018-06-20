---
layout: page
title: Automated Release Versioning
published: true
---

Manually tagging releases in GitHub and publishing release notes is tedious and error prone. Also, it can become quite labour intensive if you regularly push minor or patch releases to add improvements, correct bugs, etc. 

To combat this and help keep more accurate records when it comes to versioning, we employ an automatic release versioning system within our [build and deployment](https://university-of-york.github.io/guides/build-deployment/) processes.


## Automated versioning through Semantic Release

Our automated versioning and release system [Semantic Release](https://github.com/semantic-release/semantic-release) will process the bundles commits and automatically increment the version release number as appropriate, pushing this back to GitHub.

Versions are based on the [Semver](https://semver.org/) approach to semantic versioning, employing major, minor and patch release versions as appropriate.

### Versioning through commit message conventions 

Semantic Release does this via conventions found in git commit messages. You can read the full details on the [version control](https://university-of-york.github.io/version-control/) page, but the general gist is:

- For **patch** versions
	- fix: [title], e.g. 'fix: corrected a bug with IE'
- For **minor** versions
	- feature: [title], e.g. 'feature: added autocomplete function'
	- update: [title]
	- performance: [title]
- For **major** versions
	- BREAKING CHANGE: [description] - this should be added in the comment description, title doesn't matter

## Setting up automated release and versioning

The process is quite simple and involves the following steps to include automated versioning to your project:

**Note** this guide assumes you're using Node, yarn/npm, Webpack and building/deploying through Semaphore.

1. Install Semantic Release to the project using `yarn add semantic-release`
2. Create a JSON release config file in the project root, called `.releaserc.json`
3. Add in the default config options from the following section
4. Update the package.json file with the settings from the following section
5. Add a release entry to the package.json `scripts` section - `"release": "semantic-release"`
6. Make sure the Semaphore project settings have the `yarn release` command added

### GitHub Tokens

There needs to be a GitHub token involved for this to work. However, you shouldn't need to generate this as the correct token and environment variable is set at a global level within Semaphore CI. 

So, provided you've set up Semaphore CI properly, the versioning system will automatically pick up the authorisation token for GitHub.

### Releaserc.json config settings

The `.releaserc.json` file needs some basic settings in to ensure:

- The correct commit messages are picked up and used for versioning
- Only the `master` branch is watched for changes
- The part of the process that deploys to the Node Package Manager is **ignored**
- And that only the GitHub part of the process is applied 

```json
{
  "branch": "master",
  "verifyConditions": ["@semantic-release/github"],
  "publish": ["@semantic-release/github"],
  "npmPublish": false,
  "analyzeCommits": {
    "preset": "angular",
    "releaseRules": [
      {"type": "fix", "release": "patch"},
      {"type": "Fix", "release": "patch"},
      {"type": "update", "release": "minor"},
      {"type": "Update", "release": "minor"},
      {"type": "feature", "release": "minor"},
      {"type": "Feature", "release": "minor"},
      {"type": "performance", "release": "minor"},
      {"type": "Performance", "release": "minor"}
    ]
  }
}
```


### Package.json additional settings

To use Semantic Release effectively, the project's package.json file needs to be updated with some basic project details as follows:

```json
"name": "name of the project",
"description": "a brief description of the project",
"version": "1.0.0",
"author": "University of York Digital Team",
"license": "MIT, GPL, etc."
```