# su26-ai301-contribution_02
Implementing RMSE &amp; MAE in kstats (Kotlin)

# Phase I
Issue: https://github.com/Oremif/kstats/issues/52

Commented: Hello! I'm a student at AI301. I'd like to work on this issue, if it is not being worked on. I'm new to Kotlin, but I'd like to give it a try!

Maintainer reply: 
@sheetalsattiraju Yes, sure, thanks a lot!
If you need help, feel free to reach out

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

# Phase II
Reproduction Process
None, as it is not a bug and is a feature enhancement request.

## Environment Setup
As the issue I selected is not a bug, but a request to implement two prediction-error metrics (RMSE and MAE), this can be done directly in the kstats Kotlin Multiplatform project using Gradle — no external environment is needed since kstats has no third-party runtime dependencies. The environment setup is as follows:

1. Clone the repo
2. JDK 21 required (project uses jvmToolchain(21))
3. Run existing kstats-core tests to confirm environment works
3. ./gradlew :kstats-core:jvmTest

```kotlin// No external libraries needed — kstats-core has no runtime dependencies.
// Only kotlin.math (sqrt) is used, which is part of the Kotlin standard library.
import kotlin.math.sqrt
import kotlin.math.abs
```
Working branch: https://github.com/sheetalsattiraju/kstats/tree/52-sheetal-sattiraju-ai301

## Steps to Reproduce
None, as it is not a bug and is a feature enhancement request.

## Solution Plan
**Understand:** The issue asks for two prediction-error metrics — RMSE (Root Mean Square Error) and MAE (Mean Absolute Error) to be added to kstats-core's descriptive statistics. The solution should follow the project's existing extension-function style on DoubleArray. After adding these metrics, two test files in kstats-core/src/commonTest/ should be added as well. These are the files to modify/add, along with the .api dump files regenerated via ./gradlew apiDump.

**Match:** The existing meanAbsoluteDeviation() function is structurally similar to MAE (both average absolute deviations across an array) and can be used as a template; the existing variance()/standardDeviation() implementation is similarly close to RMSE's squared-difference-then-sqrt pattern.

**Plan:**
1. Map out the mathematical definitions: RMSE = √(Σ(actual − predicted)² / n), MAE = Σ|actual − predicted| / n.
2. Write the code as two extension functions on DoubleArray, taking a second DoubleArray parameter, using the project's typed exceptions (core/exceptions/Exceptions.kt) and shared validation helpers (core/Validation.kt) for the equal-length check instead of raw require().
3. Verify the implementation against a reference — compute the same values with NumPy/SciPy (np.sqrt(np.mean((actual-predicted)**2)), np.mean(np.abs(actual-predicted))) using the issue's example arrays and confirm they match kstats' output.
4. Provide KDoc documentation explaining each metric, its formula, and typical use cases (regression, recommender systems, forecasting), matching the style of existing functions in the module.
5. Document the tests and attach results in the GitHub issue.

**Review:** Will self-review against the project's CONTRIBUTING.md and commit message conventions before opening the PR, and confirm ./gradlew apiCheck passes.

**Evaluate:** Once the metrics are added, I will run ./gradlew :kstats-core:jvmTest to confirm the new tests pass and existing tests are unaffected, then run ./gradlew allTests to verify cross-platform (JS, Wasm, Native) behavior is identical.

### Edge cases to test:
**Input validation edge cases:**
* Mismatched actual/predicted array lengths → typed exception (per project convention, not a raw exception)
* Empty arrays → InsufficientDataException (matches the pattern already used elsewhere in kstats-core)
* Single-element arrays → should compute validly (unlike variance/skewness, RMSE/MAE don't require n ≥ 2)

**Data edge cases:**
* actual == predicted (zero error) → RMSE = 0.0, MAE = 0.0
* Large discrepancy in one element vs. many small discrepancies → confirms RMSE penalizes large errors more heavily than MAE (the core mathematical distinction between the two metrics)
* Negative values in either array → should compute normally, since both metrics operate on differences, not raw magnitudes

**RMSE/MAE-specific edge cases:**
All errors identical in magnitude → RMSE and MAE should be equal (since squaring/rooting a constant equals the constant)
Errors of mixed sign → confirms MAE correctly takes abs() and RMSE correctly squares (sign cancels) rather than allowing errors to offset each other
