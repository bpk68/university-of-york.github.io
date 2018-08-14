---
layout: page
title: Error reporting
published: true
---

Along with our [build and deployment](https://university-of-york.github.io/guides/build-deployment/) habits and [coding standards](https://university-of-york.github.io/code-reviews/) checking, we also have an integration with [Rollbar](https://rollbar.com/rob.kendal/campus-map/) for ongoing, real-time error checking of our applications. 

This guide will show you how to set this up and walk through how it works.

## Error reporting and logging

[Rollbar](https://rollbar.com) is billed as a 'post-deploy safety net' for development teams that helps with 'error monitoring for agile development and continuous delivery'. In a nutshell, Rollbar integrates into our code base and logs and reports on all errors (handled and unhandled), even notifying our Slack channels and raising GitHub issues automatically.

## Setup and integration of Rollbar into projects

Thankfully, it's quite simple to get Rollbar integrated within an app. Here are the straightforward steps involved:

1. Visit the Rollbar project dashboard and click the '+ create new project' link.
2. Choose the correct repository/project from the GitHub section drop down and click 'create'
3. On the next section, choose the primary SDK type (hint, this is most likely going to be JavaScript)
4. Follow the instructions on the next page(s) to include the code within the project and integrate Rollbar into the process.

<div class="c-alert c-alert--info" role="alert">
  <div class="c-alert__content">
    <strong>Note:</strong> there is a deployment notifications part of the process, but this is covered in more detail in our <a href="https://university-of-york.github.io/guides/build-deployment/">build and deployment</a> section
  </div>
</div>

### Project config settings

If you'd prefer a best practices version of the Rollbar integration code, you can copy the section below into a `rollbar.js` file and include it somewhere at the start of your page. 

```javascript
// include the Rollbar library from node
const RollbarLib = require('rollbar');

// set up a config object
const _rollbarConfig = {
        accessToken: '[GENERATE THIS FROM ROLLBAR]',
        captureUncaught: true,
        captureUnhandledRejections: true,
        payload: {
            environment: (process.env.NODE_ENV !== 'production') ? 'staging' : 'production',
            client: {
                javascript: {
                    source_map_enabled: true,
                    code_version: '1.0.0',
                    guess_uncaught_frames: true
                }
            }
        },
        ignoredMessages: ['(unknown): Script error.'],
        hostWhiteList: ['york.ac.uk', 'localhost']
    };

...
// later in the code, call the Rollbar init method with our config object
RollbarLib.init(_rollbarConfig);
```
