# 📋 Code Review AI Prompts and Guidelines

This document contains structured prompts and checklist items to guide comprehensive code reviews using AI tools. Use it to ensure consistency, depth, and clarity in every PR review.

> ⚠️ **Caution**: Do not attempt to submit all of these checklist items in a single prompt to an AI model. It will try to optimize for token limits and may give shallow, surface-level responses. For better results, break down your review into focused prompts (e.g., "review error handling" or "check naming conventions") to ensure depth and accuracy.


---

## 🔹 General Instruction (Apply to All Reviews)

* **Be explicit in implementation. Do not assume anything. Carefully validate all logic with clarity.**

---

## 🧠 Code Understanding

Use these prompts to build context before diving into the review:

* What does this PR do?
* What are the DB changes or queries in this PR?
* Let’s break this PR down – summarize it functionally.
* What files are most critical to review?
* What are the side effects or downstream impacts?

---

## 🧪 Functional & Logic Review

Focus on correctness, edge cases, and logic flaws:

* Are there bugs in the code?
* Identify bugs or logic errors in this PR.
* Review the code in all aspects: correctness, clarity, and standards.
* Are there edge cases we might be missing?
* Check input validations: are all user inputs safely handled?
* Review if any pre-condition checks are missing before actions.
* Are there anti-patterns in the implementation?
* Check for code smells.
* Review hardcoded values – are they justified?

---

## 🧱 Design, Maintainability & Concurrency

Check how well the code is structured and how it handles shared resources:

* Review the overall design and flow. Could it be simplified?
* Is the code modular and reusable?
* Are there unnecessary dependencies or tight couplings?
* Review concurrency – are there race conditions, shared states, or locking issues?
* Is the code maintainable: easy to read, extend, debug?

---

## 🧾 Logging & Observability

Ensure critical operations are logged meaningfully and safely:

* Review logging: Are important actions, errors, and failures logged?
* Are sensitive values present in logs?
* Is structured logging (if used) applied consistently?

---

## 🔐 Security & Validation

Protect the system from vulnerabilities:

* Are inputs validated both at the API layer and internally?
* Are authorization and permission checks enforced?
* Are secrets, tokens, or sensitive info accidentally exposed?
* Are common security issues handled (e.g., SQL injection, XSS)?

---

## 🧪 Tests

Ensure proper coverage and quality of tests:

* Are there unit and integration tests for the new code?
* Are important edge cases covered?
* Are mocks or test data realistic?
* Should this logic be tested more thoroughly?

---

## 📝 Naming, Docs & Consistency

Ensure code communicates its intent clearly:

* Review naming conventions (highlight only major issues).
* Review inline documentation – does it explain complex logic?
* Check for spelling errors in code/comments.
* Does the method `methodX()` match its functionality?
* Are there any misleading or misnamed identifiers?

---

## ⚡ Improvements & Suggestions

Elevate the code quality:

* Are there further improvements we can make beyond correctness?
* Are there repeated patterns we can refactor?
* Could this method be simplified or broken down?

---

## 📦 All-In-One Meta Prompt Template

Use this for deep full-stack PR reviews:

```
Perform a full review of this PR. Be explicit and careful. Do not assume anything.
Include functional correctness, DB steps, input validation, logging, naming, concurrency, maintainability, and security checks.
Also point out any code smells, hardcodings, missing edge cases, or anti-patterns. Summarize your findings clearly.
```

---

## 🛠️ Optional Enhancements

* Check performance implications (heavy DB queries, large loops).
* Check third-party dependency usage and necessity.
* Verify documentation updates (README, API docs, etc.) if relevant.
* Ensure backward compatibility (especially in APIs).

---

