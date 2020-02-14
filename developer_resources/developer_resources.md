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


