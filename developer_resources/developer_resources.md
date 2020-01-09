---
layout: default
title: Develper Resources
has_children: true
nav_order: 6
has_toc: false
---

## Conflicting packages

If you make changes, ensure that all gems use `openstudio-standard` shipped with OpenStudio to avoid unforeseen consequences (**do not** include an _openstudio-standard_ dependency in the gemfile).

## Generating documentation

1. Install [NodeJS](https://nodejs.org/en/)
1. Go into the repository docs file, eg. _urbanopt-scenario-gem/docs_ and type `npm install`. You must have the NREL developer VPN active to get npm install to work here. <!-- FIXME: What about outside developers? Is this restricted to NREL devs? -->
1. Move to root level of the repo, eg. _urbanopt-scenario-gem_ and enter these commands:
    1. `bundle exec rdoc --template-stylesheets ./docs/.vuepress/public/custom_rdoc_styles.css`
    1. `npm run build --prefix docs`
1. Finally, to deploy the docs, enter: `npm run deploy --prefix docs`

Rdoc options are controlled with this [config file](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/.rdoc_options).
