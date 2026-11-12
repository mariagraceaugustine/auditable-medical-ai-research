# Auditable Medical AI: Conditional Verification and Abstention

## Overview

This repository presents an exploratory research prototype investigating when an AI system should trust a medical-image classification result, request additional verification, or defer the result for review.

The study uses OrganAMNIST, an 11-class 2D CT organ-classification dataset, and two convolutional neural networks (CNNs) to investigate confidence-aware verification policies.

**Research question:** When should an AI agent trust an intermediate medical-image analysis result, and when should it seek further evidence or abstain?

## Methodology

1. Train a baseline CNN and a second CNN with a different architecture.
2. Analyze prediction confidence, classification errors, and model disagreement.
3. Investigate high-confidence errors and correlated failures.
4. Evaluate conditional verification policies that request a second prediction for selected cases.
5. Compare coverage, accuracy among accepted predictions, and verifier-call rate.

## Experimental Results

The following exploratory results were obtained on the OrganAMNIST test split (17,778 images).

| Strategy | Coverage | Accuracy among accepted predictions | Verifier-call rate |
|---|---:|---:|---:|
| Original CNN only | 100.00% | 80.33% | 0.00% |
| Always verify | 83.13% | 93.48% | 100.00% |
| Conditional verification | 84.67% | 92.22% | 52.24% |

The conditional policy invokes verification when the original CNN's confidence is below 0.65 or when it predicts class 6 or 10. If verification is invoked, a prediction is accepted only when both models agree; otherwise, it is deferred for review.

## Key Findings

- Conditional verification reduced verifier calls relative to always verifying, while maintaining 92.22% accuracy among accepted predictions.
- Model agreement was not a guarantee of correctness. Of 336 high-confidence baseline errors on the test split, disagreement flagged 130 and missed 206.
- The models exhibited correlated errors, and reliability differed substantially between validation and test data.
- A separate validation-selected policy achieved 95.52% accuracy among accepted predictions on validation data, but only 89.57% on the test split, illustrating generalization limitations.

## Limitations

- This is an exploratory research prototype, not a clinically validated system.
- The task is 2D organ classification, not segmentation or longitudinal tumour-response assessment.
- Some policy development was informed by earlier test-set analysis; results should not be interpreted as an unbiased final generalization estimate.
- Softmax confidence does not guarantee calibrated uncertainty.
- Both CNNs share training data and can make the same incorrect prediction.
- Verifier-call rate is a computational proxy, not measured runtime or clinical cost.
- Exact-image duplicate checks do not establish patient-level independence.

## Future Research

Future directions include more robust uncertainty estimation, independent verification strategies, evaluation under dataset shift, auditable decision policies, and extension to longitudinal medical-image analysis.

## Code Availability

This public repository contains the research overview and experimental findings only. The Python implementation, trained model weights, and datasets are not publicly shared. Researchers interested in reviewing the implementation may contact the author to request access.

## Author

Maria Grace Augustine

MSc Embedded Systems Engineering | Machine Learning and Computer Vision | Reliable AI

## Dataset

OrganAMNIST from the MedMNIST benchmark: https://medmnist.com/
