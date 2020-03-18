---
layout: default
title: Adding your own Measures
parent: Customization
nav_order: 2
---

Additional Measures can be added to the [base workflow OSW](base_workflow.md) file by adding the Measure name and directory name along with the Measure arguments.  If the Measure resides in one of the URBANopt core gems, then no other change is needed.  If the Measure resides in another gem not included in the URBANopt core gems, then the gem must be included in the project Gemfile.  If the Measure is new or just not in a gem, add the path to the Measure in the workflow OSW file's "measure_paths" array.