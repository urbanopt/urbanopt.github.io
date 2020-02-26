---
layout: default
title: Developer Resources
has_children: true
nav_order: 6
has_toc: false
---

# Development Process
1. Have CI added to any new SDK repo
1. Write tests for new code functionality. Run the tests locally
1. Make sure there is an issue created for items you are working on (for tracking purposes and so that the item appears in the changelog for the release). Use the following labels on the GitHub issue:
	1. **Feature** (features will appear as “New” item in the changelog)
	1. **Enhancement** (these will appear as “Improved" in the changelog)
	1. **Bug** (these will appear as “Fixed” in the changelog)

1. Branch off of the `develop` branch (unless it is a hotfix for production) 
1. When done, create pull requests against the `develop` branch. You can group related issues together in the same PR).  
	1. Make sure all tests are passing and run rubocop
	1. Assign a reviewer to look over the code
	1. Use the “DO NOT MERGE” label for Pull Requests that should not be merged

## Conflicting packages

If you make changes, ensure that all gems use the  `openstudio-standard` gem shipped with OpenStudio to avoid unforeseen consequences (**do not** include an _openstudio-standard_ dependency in the gemfile).


## Generating rdoc Documentation
Follow these steps to generate rdocs for now and existing SDK gems:
1. Install [NodeJS](https://nodejs.org/en/)
1. Go to the repository docs file, eg. _urbanopt-scenario-gem/docs_ and type `npm install`. Note: access to developerVPN will be needed for this step if within the NREL firewall.
1. Move to the root level of the repo, eg. _urbanopt-scenario-gem_ and enter these commands:
	```
    bundle exec rdoc --template-stylesheets ./docs/.vuepress/public/custom_rdoc_styles.css
    npm run build --prefix docs
    ```
1. Finally, to deploy the docs, enter: `npm run deploy --prefix docs`

Rdoc options are controlled with this [config file](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/.rdoc_options).

## Developing with the CLI

When developing locally on URBANopt core gems and testing new functionality and dependencies via the CLI, it may be necessary to install local versions of core gems.  

For local development, you will want to set the environment variable *FAVOR_LOCAL_GEMS* to 1. This enables local copies of gems in the Gemfile.  Note that setting *FAVOR_LOCAL_GEMS* to 0 will not undo local gems functionality: you will have to either remove the *FAVOR_LOCAL_GEMS* environment variable, set it to *nil* or *false* (not 0), or open a new terminal window to turn it off.  

There are 2 Gemfiles of interest:

1. When testing a new measure that will run in the OpenStudio workflow OSW file, enable the gem that contains the measure in the Gemfile of the project you are testing by uncommenting the appropriate block of code. You may have to modify the path or branch referenced to match what you are testing.

1. When testing new functionality in the URBANOPT workflow (a new scenario post-processor, for example), enable the gem in the Gemfile of the URBANopt-cli local directory by uncommenting the appropriate block of code. You may have to modify the path or branch referenced to match what you are testing.

Follow these steps when testing local gems via the CLI:

1. ```bundle install``` (to install the local gems that were just enabled in the Gemfile)
1. ```bundle exec rake install``` (to install the CLI in a place that has access to those local gems)
1. ```bundle exec uo ...``` (to execute the CLI calls)

## Example Project Development

1. Develop new functionality in the example project directory.  
1. When the functionality is ready and all tests are passing, update the example project files in the urbanopt-cli repo.

