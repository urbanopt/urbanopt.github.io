---
layout: default
title: For Developers
has_children: true
nav_order: 9
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

## Functionality Development / Copying over to the CLI

Functionality should be developed in the urbanopt-example-geojson-project repo. Once it is ready and passing tests, the files from urbanopt-example-geojson-project will be copied over to the urbanopt-cli repo. Take care when performing the copy process as it is possible some changes were made directly on the cli repo (i.e., you might have to diff the files and reconcile both repos with the latest changes).

Notes for the copy process (from example-project repo to cli repo):

1. Copy example_project/resources folder to example_files/resources
1. Copy mappers folder over. **Note:** take the residential folder that is within the mappers folder and put it next to the mappers folder in the CLI repo
1. Copy base_workflow.osw again and place it next to the mappers folder. Rename it as base_workflow_res.osw and place it NEXT TO the mappers folder (not inside).
1. Open mappers/base_workflow.osw file in cli repo (you have previously copied it over in step 2) Remove the "BuildResidentialModel" measure section completely from this file and save.
1. copy the measures folder to example_files/measures
1. copy xml_building to example_files/xml_building
1. copy osm_building to example_files/osm_building
1. copy reopt to example_files/reopt. (on this one maybe diff the files to ensure that changes weren't just made on the CLI repo)
1. copy visualization to example_files/visualization
1. There is no need to copy any of the scenario CSVs

## Conflicting packages

If you make changes, ensure that all gems use the `openstudio-standards` gem shipped with OpenStudio to avoid unforeseen consequences (**do not** include an _openstudio-standards_ dependency in the gemfile).

## Generating rdoc Documentation

Follow these steps to generate rdocs for new and existing SDK gems:

1. Install [NodeJS](https://nodejs.org/en/)
1. Go to the repository docs file, eg. _urbanopt-scenario-gem/docs_ and type `npm install`
1. Move to the root level of the repo, eg. _urbanopt-scenario-gem_ and enter these commands:

```bash
    bundle exec rdoc --template-stylesheets ./docs/.vuepress/public/custom_rdoc_styles.css
    npm run build --prefix docs
```

1. Finally, to deploy the docs, enter: `npm run deploy --prefix docs`

Rdoc options are controlled with this [config file](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/.rdoc_options).

## Developing with the CLI

When developing locally on URBANopt<sup>&trade;</sup> gems and testing new functionality and dependencies via the CLI, it may be necessary to install local versions of some gems.

For local development, you will want to set the environment variable _FAVOR_LOCAL_GEMS_ to 1. This enables local copies of gems in the Gemfile.  Note that setting _FAVOR_LOCAL_GEMS_ to 0 will not undo local gems functionality: you will have to either remove the _FAVOR_LOCAL_GEMS_ environment variable or set it to _nil_ or _false_ (not 0), and open a new terminal window to turn it off.

There are 2 Gemfiles of interest:

1. When testing a new measure that will run in the OpenStudio workflow OSW file, enable the gem that contains the measure in the Gemfile of the project you are testing by uncommenting the appropriate block of code. You may have to modify the path or branch referenced to match what you are testing. If you are adding a new OpenStudio measure gem, you will also need to (temporarily) add it to the CLI gemfile until you have released the measure gem on rubygems.org.  Follow the instructions below to install the CLI in development mode once you have the gem added to the CLI Gemfile.

1. When testing new functionality in the URBANOPT workflow (a new scenario post-processor, for example), enable the gem in the Gemfile of the URBANopt-cli local directory by uncommenting the appropriate block of code. You may have to modify the path or branch referenced to match what you are testing.

Follow these steps when testing local gems via the CLI:

1. ```bundle install``` (to install the local gems that were just enabled in the Gemfile)
1. ```bundle exec rake install``` (to install the CLI in a place that has access to those local gems)
1. ```bundle exec uo ...``` (to execute the CLI calls)

## Example Project Development

1. Develop new functionality in the example project directory.
1. When the functionality is ready and all tests are passing, update the example project files in the urbanopt-cli repo.
