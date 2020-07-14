---
layout: default
title: REopt Customization
parent: REopt
nav_order: 3
---
## Customization

Advanced developers may choose to customize the **URBANopt REopt Gem**. For example, more attributes are returned from the **REopt Lite API** and saved to disk than current can be recorded in a Feature or Scenario Report. In these cases, developers will want to clone the [URBANopt REopt Gem](https://github.com/urbanopt/urbanopt-reopt-gem) and possibly the [URBANopt Scenario Gem](https://github.com/urbanopt/urbanopt-scenario-gem) and make modifications to these codebases. Such changes can be made accessible through CLI commands (installed from a cloned URBANopt CLI repo) or a local project. See [Developer Resources](../developer_resources/developer_resources.md) for more information about setting up local gems and installing them through modifications to your project's Gemfile.

### Adding additional REopt Lite Responses to your Feature and Scenario Reports

To include additional **REopt Lite API** responses to your Feature and Scenario Reports follow the steps we outline below for adding optimal lifecycle costs from a **REopt Lite API** response to Feature and Scenario Reports.

(1) We'll need to update the Feature and Scenario Report schema to allow for new attributes, so clone [URBANopt Scenario Gem](https://github.com/urbanopt/urbanopt-scenario-gem) to your local machine.

`git clone https://github.com/urbanopt/urbanopt-scenario-gem`

(2) Later we'll need to also update the [URBANopt REopt Gem](https://github.com/urbanopt/urbanopt-reopt-gem) to parse these additional attributes from the **REopt Lite API** response, so clone this now in the same directory as the **URBANopt Scenario Gem** repository with:

`git clone https://github.com/urbanopt/urbanopt-scenario-gem`

(3) Let's first update the **URBANopt Scenario Gem**. The `distributed_generation` schema used by Feature and Scenario Reports is defined in this repository and needs to handle new attibutes. We'll add `lcc_us_dollars` here as an example. 

  a. Open `urbanopt-scenario-gem/lib/urbanopt/scenario/default_reports/distributed_generation.rb`

  b.  In the `DistributedGeneration` class add a new attribute accessor:
```ruby
module URBANopt
    module Scenario
      module DefaultReports
      ##
      # Onsite distributed generation system (i.e. SolarPV, Wind, Storage, Generator) design attributes and financial metrics.
      ##
      class DistributedGeneration
        ##
        # _Float_ - Lifecycle costs for the complete distributed generation system in US Dollars
        #
        attr_accessor :lcc_us_dollars
```

  c. Within the `initialize` function parse the attribute when the object is instantiated:
```ruby
        def initialize(hash = {})
          hash.delete_if { |k, v| v.nil? }
          @lcc_us_dollars = hash[:lcc_us_dollars]
```


  d. Within the `to_hash` function, make sure the attribute will be exported when the report is saved: 
```ruby    
        def to_hash
          result = {}
          result[:lcc_us_dollars] = @lcc_us_dollars if @lcc_us_dollars
```    
    
  e. Within the `merge_distributed_generation` function, make sure the attribute will be added when two reports are combined: 
```ruby
        def self.merge_distributed_generation(existing_dgen, new_dgen)
           existing_dgen.lcc_us_dollars = add_values(existing_dgen.lcc_us_dollars, new_dgen.lcc_us_dollars)
```

  You're now all set updating the `distributed_generation` schema.

4. Let's next update **URBANopt REopt Gem** to load lifecycle costs from the **REopt Lite API** responses into the Feature and Scenario Report `distributed_generation` schema's `lcc_us_dollars` attribute.
  
a. First, in `urbanopt-reopt-gem/Gemfile` make sure that the `urbanopt-scenario` gem is loaded as follows:
    
`gem 'urbanopt-scenario', path: '../urbanopt-scenario-gem'`

This will ensure that the `urbanopt-sceanrio` gem that the `urbanopt-reopt` gem uses includes the modified schema. 

_Note:_ You can alternatively specify a gem from a branch on github as follows:

`gem 'urbanopt-scenario', github: '<repo address>', branch: '<branch name>''`

  b. Next, in `urbanopt-reopt-gem/lib/reopt/feature_report_adapter.rb` in the `update_feature_report` function, parse the **REopt Lite API** response:
 
 ```ruby
    # Update distributed generation sizing and financials
    feature_report.distributed_generation.lcc_us_dollars = reopt_output['outputs']['Scenario']['Site']['Financial']['lcc_us_dollars'] || 0
```

  For an example of a **REopt Lite API** response refer to this [ file](https://github.com/urbanopt/urbanopt-reopt-gem/blob/develop/spec/run/example_scenario/reopt/scenario_report__reopt_run.json). Also, the REopt API ouput schema is documented [here](https://developer.nrel.gov/docs/energy-optimization/reopt-v1/#Scenariooutputs_panel).

  c. Finally, in `urbanopt-reopt-gem/lib/reopt/scenario_report_adapter.rb` in the `update_scenario_report` function, parse the lifecycle cost from the **REopt Lite API** response:

 ```ruby
    # Update distributed generation sizing and financials
    scenario_report.distributed_generation.lcc_us_dollars = reopt_output['outputs']['Scenario']['Site']['Financial']['lcc_us_dollars'] || 0
 ```
 
  d. You are now ready to navigate to the `urbanopt-reopt-gem` folder and run:

 ```bash
  $ bundle install
  $ bundle update`
```

5. Now, when you reference this modified `urbanopt-reopt` implementation you can access the lifecycle costs from `feature_report.distributed_generation.lcc_us_dollars` or `scenario_report.distributed_generation.lcc_us_dollars`. The lifecycle costs will also be included when a Feature or Scenario Report is saved. 
