# yAcademy Spartan ECDSA Review <!-- omit in toc -->

**Review Resources:**

None beyond the code repositories.

**Auditors:**

- Chen Wen Kang
- Vincent Owen T

## Table of Contents <!-- omit in toc -->

- [Review Summary](#review-summary)
- [Scope](#scope)
- [Findings Explanation](#findings-explanation)
- [Critical Findings](#critical-findings)
- [High Findings](#high-findings)
- [Medium Findings](#medium-findings)
- [Low Findings](#low-findings)
- [Informational Findings](#informational-findings)
  - [1. Informational - Remove Uneeded Variable "Bits"](#1-informational---remove-uneeded-variable-bits)
    - [Technical Details](#technical-details)
    - [Impact](#impact)
    - [Recommendation](#recommendation)
  - [2. Informational - Constraints for `add.cicom` for values to be non-zero](#2-informational---constraints-for-addcicom-for-values-to-be-non-zero)
    - [Technical Details](#technical-details-1)
    - [Impact](#impact-1)
    - [Recommendation](#recommendation-1)
  - [3. Informational - More tests for the circuits](#3-informational---more-tests-for-the-circuits)
    - [Technical Details](#technical-details-2)
    - [Impact](#impact-2)
    - [Recommendation](#recommendation-2)
- [Final remarks](#final-remarks)

## Review Summary

**Spartan ECDSA**

The circuits of the Spartan ECDSA [Repo](https://github.com/personaelabs/spartan-ecdsa) were reviewed over 5 days. The code review was performed by 2 auditors between 1 Jul and 5 Jul, 2023.

## Scope

The scope of the review consisted of the following circuits at the specific commit `5dae5e1aa4eda726ddffc08eaec0144d003a98a0`:

- `eff_ecdsa.circom`
- `tree.circom`
- `add.circom`
- `mul.circom`

This review is a code review to identify potential vulnerabilities in the code. The reviewers did not investigate security practices or operational security and assumed that privileged accounts could be trusted. The reviewers did not evaluate the security of the code relative to a standard or specification. The review may not have identified all potential attack vectors or areas of vulnerability.

yAcademy and the auditors make no warranties regarding the security of the code and do not warrant that the code is free from defects. yAcademy and the auditors do not represent nor imply to third parties that the code has been audited nor that the code is free from defects. By deploying or using the code, Spartan ECDSA and users of the circuits agree to use the code at their own risk.

## Findings Explanation

Findings are broken down into sections by their respective impact:
 - Critical, High, Medium, Low impact
     - These are findings that range from attacks that may cause loss of funds, impact control/ownership of the circuits, or cause any unintended consequences/actions that are outside the scope of the requirements
 - Informational
     - Findings including recommendations and best practices

---

## Critical Findings

None.

## High Findings

None.

## Medium Findings

None.

## Low Findings

None.

## Informational Findings

### 1. Informational - Remove Uneeded Variable "Bits"

Found an uneeded variable in the EfficientECDSA circuit. The bits variable is not needed in this circuit because the variable is not being used anywhere in the circuit.

#### Technical Details

```jsx
template EfficientECDSA() {
    var bits = 256;
    signal input s;
    signal input Tx; // T = r^-1 * R
    signal input Ty;

```

#### Impact

Informational.

#### Recommendation

```jsx
template EfficientECDSA() {
    signal input s;
    signal input Tx; // T = r^-1 * R
    signal input Ty;
```

### 2. Informational - Constraints for `add.cicom` for values to be non-zero

In signal assignments containing division, the divisor needs to be constrained to be non-zero. (circomspect)

#### Technical Details

```
   │
31 │     lambda <-- dy / dx;
   │                     ^^ The divisor `dx` must be constrained to be non-zero.
```

#### Impact

Informational.

#### Recommendation

Do an additional check for non-zero values.


### 3. Informational - More tests for the circuits


Tests such as negative tests are always good to have in order to cover more unexpected cases.

#### Technical Details

`eff_ecdsa.test.ts` and `eff_ecdsa_to_addr.test.ts` only have 1 positive tests.

#### Impact

Informational.

#### Recommendation

Adding more tests for the circuits.

## Final remarks

Spartan ECDSA does not have any major issues. The circuits are well documented.
