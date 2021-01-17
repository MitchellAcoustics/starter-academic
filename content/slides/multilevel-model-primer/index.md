---
title: Multilevel Modelling Primer
summary: A Primer on Multi-level Regression Modelling
authors: [admin]
tags: [lectures]
categories: [Machine Learning]
date: "2021-01-17"
slides:
  # Choose a theme from https://github.com/hakimel/reveal.js#theming
  theme: black
  # Choose a code highlighting style (if highlighting enabled in `params.toml`)
  #   Light style: github. Dark style: dracula (default).
  highlight_style: dracula
marp: true
---

<!-- theme: default -->
<!-- class: invert -->

# A Primer on Multi-level Regression Modelling

Andrew Mitchell

---

## What is a multi-level model?

{{% fragment %}}
- Also known as hierarchical, linear mixed effects regression (LMER), or linear mixed models (LMM) 
{{% /fragment %}}
{{% fragment %}}
- Useful for clustered data, repeated measures data, longitudinal data
{{% /fragment %}}
{{% fragment }}
- Examples:
  - voting data by states
  - animal weight over time
  - radon intrusion into homes sorted into counties and states
  - students in different schools
{{% /fragment }}

{{% speaker_note %}}
We are already familiar with multiple linear regression - a model in which multiple input features are considered and each gets their own coefficient. But these coefficients are fitted and defined globally, for the entire dataset. What if we have data which is sorted into separate groups, where those relationships between the input and output features may differ.

Consider an education study with data from students in many schools, predicting in each school the students' grades *y* on a test, given their score on a pre-test and other information. The relationship between the pre-test score and the final test score may vary depending on what school the student is in -> maybe one school focussed on improving between the two, maybe another school had a change of staff. How would our standard model account for this? It can't, because it's only considering the general trend between the pre-test and final test scores.

One solution to this may be to build a separate model for each school. 
{{% /speaker_note %}}

---
