---
layout: default
title: Running URBANopt Scenarios in Parallel (Local Execution)
parent: Workflows
nav_order: 6
---

# Running large-scale building simulations locally in URBANopt 
    
URBANopt supports **parallel execution of building simulations** during a Scenario run, allowing users to significantly reduce total runtime when simulating multiple buildings locally. Parallelization is handled at the **building level** and leverages multiple CPU cores on the local machine.

## Prerequisites

Before running URBANopt simulations in parallel, ensure that:
    - URBANopt CLI is installed and available on your system path.
- EnergyPlus and OpenStudio dependencies are correctly installed.
- Your machine has sufficient CPU cores and memory to support parallel execution.
- The Scenario is configured with multiple buildings (Feature files).

    > **Note:** Parallel execution increases memory usage. Users should balance the number of parallel workers with available system resources.
    
## Parallel Simulation Overview

When running a Scenario, URBANopt simulates each building independently. By enabling parallel execution, these building simulations can be distributed across multiple worker processes, allowing several buildings to be simulated simultaneously.

## Configuring Parallel Execution

Parallel execution is controlled through URBANopt CLI command-line options, by editing the `runner.conf` file or by creating a num_parallel environment variable.

### 1. Using the URBANopt CLI

#### Key Option: Number of Workers

Use the `--num-workers` flag to specify how many parallel worker processes should be used:

```bash
uo run --num-workers N
```    
Where:

• N is the number of parallel workers

• Each worker typically corresponds to one CPU core

Example

    To run a Scenario using 4 parallel workers:
    uo run --num-workers 4
    This command will:
        • Launch up to 4 building simulations concurrently
        • Queue remaining buildings until a worker becomes available
    
### 2. Editing the input configuration file

In the runner.conf file, update the `"num_parallel"` field to reflect the updated number of models to run in parallel

### 3. Setting an environment variable

A user can also create an environment variable and set a global num-parallel value. To do this, create an environment variable and set it to the number of cores you want to use: UO_NUM_PARALLEL=7 or other appropriate number. More details are provided on the [getting started page](../getting_started/getting_started.md).

## Choosing the Number of Workers
    A good rule of thumb is:
        • Start with (number of CPU cores - 1) workers
    Reduce the number of workers if you encounter:
        • Out-of-memory errors
        • Excessive system slowdown
        • EnergyPlus crashes
    Examples
        • 8-core machine : try --num-workers 6
        • 16-core machine : try --num-workers 12
    
    Monitoring Parallel Runs:
    During execution, the CLI output will indicate:
        • Which buildings are being simulated
        • When individual simulations start and complete
        • Any errors encountered by worker processes
    Each building’s simulation results are written to its own output directory, allowing partial results to be inspected even if other buildings fail.
    
    Output Structure:
    Parallel execution does not change the URBANopt output structure. Results are written as usual to the Scenario run directory, including:
        • Per-building simulation outputs
        • Scenario-level aggregation results
        • REopt and ERP outputs (if enabled)
    
    Common Issues and Troubleshooting:

    High Memory Usage
    
    If simulations fail or the system becomes unresponsive:
        • Reduce --num-workers
        • Close other memory-intensive applications
    Non-Deterministic Completion Order
    Buildings may complete in a different order across runs. This is expected behavior and does not affect results.
    Debugging Failures
    If a building simulation fails:
        • Navigate to the individual building output directory
        • Review EnergyPlus and OpenStudio log files
        • Consider rerunning with fewer workers or a single worker for debugging
    
    Best Practices:
        • Use parallel execution for large scenarios with many buildings.
        • Use single-worker runs (--num-workers 1) when debugging measure logic or model setup.
        • Gradually increase workers to find the optimal balance for your machine.

