---
layout: default
title: Urban Systems Generator
parent: Residential Workflows
grand_parent: Workflows
nav_order: 4
---

# URBANopt&trade; - Urban Systems Generator (USG) Integration

**Alpha Version** - This documentation covers the initial test capability for URBANopt-USG integration. The current implementation provides basic data gap filling for URBANopt residential modeling workflows using a single generalized machine learning (ML) model. Future versions may also address URBANopt commercial building workflows and may include localized models, generative artificial intelligence (AI) for scenario generation, and advanced multi-modal capabilities.  

## Overview

The Urban Systems Generator (USG) integration streamlines district-scale energy modeling in URBANopt by addressing the core challenge of data-intensive bottom-up model setup. USG uses ML and AI models to fill data gaps in building characteristics by predicting missing URBANopt-BuildStock parameters required for simulation. 

This integration leverages NLR’s BuildStock dataset to train data-driven models that learn the relationships between building characteristics and their direct mappings. This model is utilized here to infer missing characteristics from a set of known characteristics to characterize a baseline URBANopt model. 

