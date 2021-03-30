---
layout: default
title: Release Instructions
parent: For Developers
nav_order: 2
---

## URBANopt release process

The modular nature of URBANopt<sup>&trade;</sup> allows for freedom in developing and using only the parts relevant to your work. Keep in mind the following dependencies:

![gem dependency chart](../doc_files/uo_dependency_rank.png)

We recommend releasing gems **in order from the base to most dependent**. For each gem being improved, follow these steps:

1. Increment version (if needed) in `/lib/*/version.rb`
1. For gems with measures in them, run the following rake tasks and commit the changes:
	```
	rake openstudio:test_with_openstudio
	rake openstudio:update_measures
	```
1. Run `RuboCop` on all PRs before merging to `develop`
    ```
	rake rubocop:auto_correct
	```
1. Remove .DS_Store files if any are in the repo
1. If the gem has rdoc documentation, [regenerate the rdocs](../developer_resources/developer_resources.md#generating-rdoc-documentation)
1. Run the changelog rake task and add the changes to the CHANGELOG file for the range of time between last release and this release. Also make sure that all pull requests have a related Issue to be included in the change log.
	```
	rake openstudio:change_log[start_date,end_date,apikey]
	```
    No spaces around the commas! Paste the `Closed Issues` into the CHANGELOG, matching formatting as appropriate.
1. Merge pull requests to the `develop` branch
1. Create PR to master
    - Ensure all tests pass before merging
1. Locally - from the master branch, run `rake release` to release the gem to RubyGems
1. Update the documentation with detailed usage and helpful examples
1. On GitHub, go to the releases page and update the latest release tag. Name it “Version x.y.z” and copy the CHANGELOG entry into the text box.
    - Link to relevant documentation URLs in release tags
1. Update [Compatibility Matrix](compatibility_matrix.md) as appropriate
