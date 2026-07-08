# su26-ai301-contribution_02
Implementing RMSE &amp; MAE in kstats (Kotlin)

# Phase I
Issue: https://github.com/Oremif/kstats/issues/52

Commented: Hello! I'm a student at AI301. I'd like to work on this issue, if it is not being worked on. I'm new to Kotlin, but I'd like to give it a try!

No maintainer reply yet, but active repo.

## Why I Chose This Issue
I chose issue #52 because I have experience in these types of regression metrics from work and prior school projects. However, I have not worked with Kotlin before, but I have seen C-type syntax before and would like to get comfortable working in this syntax. The issue is labelled as "good for new contributers", so it aligns with my few experiences in open source contribution. 

## Problem summary
The maintainer asked for prediction error metrics to be added to kstats-core descriptive statistics. Those statistics being:
* RMSE (Root Mean Square Error) = √(Σ(actual - predicted)² / n)
* MAE (Mean Absolute Error) = Σ|actual - predicted| / n
These are used for evaluating prediction quality in regression, recommendation systems, and forecasting. Since this request is an enchancenment/ feature request, nothing is "broken" but the value for these metrics is helping any user using this library an efficient way to verify their models are working. 

I'm interested in this because:
1. I've worked with these types of metrics before in personal projects, so I understand how to create and verify them.
2. I'm interested in working with C-type syntax languages, as I primarily focus in Python and don't get enough exposure.
3. The maintainer's general activity indicate they're actively reviewing PRs and creating new issues.

## What fixed should look like:
- Only these files need to be modified/ created:
  - metric files such as: rmse.kt and mae.kt in folder strucutre: https://github.com/Oremif/kstats/tree/master/kstats-core/src/commonMain/kotlin/org/oremif/kstats/descriptive
  - test file such as: rmse_test.kt and mae_test.kt in folder structure: https://github.com/Oremif/kstats/tree/master/kstats-core/src/commonTest/kotlin/org/oremif/kstats/core 
- Fixed: adding RMSE and MAE in kstats library via Kotlin after verifying all tests have been passed.
