# Community Contribution Process

## Community Contribution Process

### GitHub Issue/Jira–Driven Contribution Model:

### Purpose

This document describes the community contribution process agreed for accepting external contributions into monitored GitHub repositories.

The objective of this process is to:

- Enable community contributions in a controlled and secure manner
- Ensure consistency, traceability, and quality across repositories
- Address CI, security, and compliance constraints inherent to fork-based contributions
- Ensure an open, inclusive, and well‑structured experience that makes collaboration smooth for contributors and maintainers alike

GitHub Issues are the recommended entry point for community contributions because they provide a convenient way to discuss proposals, track progress, and link related pull requests.

Contributors who are more comfortable with the existing Jira-based process may continue to raise a Jira ticket as an alternative. Both approaches are supported, provided the contribution details are documented clearly, and the related pull requests are linked to the tracking item. Jira space: https://jira.rdkcentral.com/jira/projects/RDKBDEV/

**Recommendation:** New contributors are encouraged to use the GitHub Issue-based process. Contributors who already use the Jira workflow are welcome to continue using it.

---

## Guiding Principles

- Community contributors do not have write access to the RDKCENTRAL repositories.
- All contributions must be tracked, reviewed, and validated before merging into develop branch.
- CI, security scans, and integration builds must run only on trusted branches.
- The process should remain predictable, auditable, and scalable as contribution volume grows.
- GitHub issues will be the standard way of tracking GitHub contributions.
- GitHub Issues are the preferred way to initiate and track community contributions. Jira remains an accepted alternative for contributors who are more comfortable with the existing Jira-based process.
- Regardless of the entry point, every contribution should have a clearly identifiable tracking item, and all related pull requests should be linked to it.

---

## Entry Point: GitHub Issue

Every community contribution starts with the creation of a GitHub Issue using a standardized Issue Form.

The GitHub Issue serves as:

- The formal request for a contribution
- The agreement on scope, base tags, and repositories involved
- The tracking mechanism throughout the contribution lifecycle

All contributions must begin with a GitHub Issue that is reviewed and approved before any code is submitted. This ensures early alignment on scope, ownership, and expectations.

---

## Community Issue Repository for Contributions

To simplify the contribution process and provide consistent tracking, the community uses the `rdkcentral/rdk-community-contributions` repository as the recommended entry point for GitHub-based contributions.

Contributors following the GitHub workflow should create their contribution issue in this repository.

**rdkcentral/rdk-community-contributions**

This repository serves as the single-entry point and system of record for contribution tracking, regardless of how many component repositories are involved.

Contributors who prefer the existing Jira-based workflow may create and maintain their contribution ticket in Jira instead. In either case, please ensure that the contribution details are complete and that the tracking item can be easily identified throughout the review process.

---

## Why a Community Issue Repository Is Used

Using a single, default anchor repository provides the following benefits:

- A clear and predictable starting point for all contributors
- Centralized triage, prioritization, and discussion
- Easier cross-repository coordination and review
- Reduced duplication of issues across repositories

---

## How Contributions Are Tracked Using the Community Issue Repository

For contributions following the GitHub Issue-based process:
- One GitHub Issue is created in the rdk-community-contributions repository
- This issue captures all the required mandatory fields
- All pull requests raised in individual component repositories must reference this GitHub issue
- Example: “Related to rdkcentral/rdk-community-contributions#123”

For contributors following the Jira-based process:
- Create a Jira ticket containing the contribution details, proposed scope, and all mandatory information.
- Link all related pull requests to the Jira ticket, and include the ticket reference in the PR description.
- Keep the ticket updated as the contribution progresses through review, testing, and merging.

Whichever approach you choose, the goal is the same: maintain clear traceability between the original contribution request, the implementation, review feedback, and the final outcome.

---

## Contributions Involving Multiple Repositories

For contributions that span multiple repositories:

- Do not create separate issues in each component repository
- Use the same GitHub issue to track the entire contribution
- Link all related pull requests back to the GitHub issue

This ensures reviewers and maintainers have a complete, end-to-end view of the change.

---

## Friendly Reminder to Contributors

Creating the issue in the rdk-community-contributions repository helps the Contribution Intake Team respond faster and more consistently.

---

## Types of Contributions

### Bug Fixes

Community members are encouraged to fix bugs and help improve the stability and reliability of the project.

Bug GitHub Issues are reviewed by the Contribution Intake Team. Once validated, contributors are invited to submit a fix.

Please note: Submitting a bug fix does not automatically guarantee immediate resolution. All bug issues are reviewed, and acceptance is based on technical validity, impact, and project priorities.

Contributors are welcome to provide fixes, test cases, or additional diagnostics to help move the issue forward.

---

### Feature Contributions

Community members are welcome to propose new features that help enhance and evolve the project. We value ideas that improve functionality, usability, performance, or long-term maintainability.

Feature requests are reviewed during RTAB meetings and considered alongside the project roadmap and available community bandwidth. Active participation and familiarity with the project can help reviewers better assess feasibility and alignment.

Please note: Submitting a feature request does not guarantee acceptance or prioritization.

---

### Improvement & Enhancement Contributions

Improvement and enhancement contributions such as refactoring, code simplification, documentation updates, build improvements, or removal of unused code—are highly appreciated. These contributions help keep the project healthy, maintainable, and easier for everyone to work with.

Cleanup contributions are reviewed to ensure they do not introduce regressions or unintended side effects. In some cases, reviewers may suggest breaking large cleanups into smaller, incremental changes to make review easier.

Please note: Even though cleanup contributions may not add new features, they still follow the same review and approval process to maintain project quality.

Contributors are encouraged to include tests, documentation updates, or before-and-after explanations where applicable.

---

## GitHub Issue Template – Required Fields

To help reviewers quickly understand your contribution, we encourage you to provide the details listed below. Unless explicitly marked optional, these fields give the necessary context for triage, review, and merging.

### Mandatory Fields

| Field Name | Field Description |
|----------|------------------|
| Type of Contribution | Select the issue type that best describes your contribution:<br>• Bug Fix<br>• Feature<br>• Improvement/Enhancement |
| Summary | A short, clear summary describing the contribution. This helps reviewers quickly understand the intent. |
| Description | A detailed explanation of the problem being addressed or the enhancement being proposed. Provide sufficient context for reviewers. |
| Repositories Involved | List all GitHub repositories impacted by this contribution, including any related meta-layers or manifests. |
| Base Tag for Repositories | Specify the base branch or tag for each repository involved in the contribution.<br><br>As a general guideline, contributors are encouraged to use the default or currently active development branch for the repository (for example: main or develop), unless there is a specific reason to target a different branch or release tag.<br><br>Friendly Note to Contributors: If you are unsure which base branch or tag to use, start with the default or active branch—the Contribution Intake Team will guide you during review if an adjustment is required. |
| Steps to Reproduce / Acceptance Criteria | This section helps reviewers understand the expected behavior and validate the contribution effectively. The level of detail depends on the contribution type:<br><br>For bug fixes: Provide clear, repeatable steps to reproduce the issue so reviewers can confirm the problem and verify the fix.<br><br>For feature contributions: Define clear acceptance criteria describing when the feature should be considered complete. This may include functional behavior, edge cases, and expected outputs.<br><br>For improvement or enhancement contributions: Describe the current behavior or structure and explain what is being improved. Where applicable, include:<br>• What is changing and what is intentionally not changing<br>• Expected improvements (e.g., readability, maintainability, performance)<br>• How reviewers can validate that existing behavior remains unchanged.<br><br>If the change is behavior-neutral, please explicitly state that there is no functional impact. |
| Test Cases | Describe the test scenarios used to validate the change. Include both positive and negative cases where applicable. |
| Expected Result | Describe the expected behavior after the change is applied. |
| Actual Result | Required for bug reports. Describe the current or incorrect behavior observed before the fix. |
| Test Reports | Provide links or attachments to any available test results. Screenshots or logs are encouraged where relevant. |

---

## Mandatory Labels

- Labels: community-contribution.
  Contributors must ensure that the community-contribution label is applied to the GitHub Issue when it is created.

---

## Contribution Workflow:

## Simplified Fork-Based PR Workflow (Default)

> **Purpose:**  
> This workflow is the **default and recommended approach for small, low‑risk contributions**, such as:
> - Bug fixes
> - Documentation updates
> - Code cleanups or formatting changes
> - Minor refactoring with no functional impact
>
> For feature development or larger efforts requiring extended design, testing, and coordination, refer to the **Standard Development Workflow**.

---

### Overview

The Simplified Fork-Based PR Workflow allows contributors to make isolated changes using a forked repository while ensuring code quality through builds and structured reviews.

This workflow is intentionally lightweight to enable faster turnaround while maintaining necessary quality gates.


```mermaid
flowchart TD
    A[Contributor] --> B[Create GitHub Issue in<br/>rdk-community-contributions]

    B --> C[Fork Code Repository]

    C --> D[Raise PR from Fork<br/> to the development branch<br/>&Link GitHub Issue]

    D --> E[Contribution Intake Team]

    E --> F[Create PR Builds]

    F --> G[Execute Required Testing]

    G --> H[Share Results with<br/>Component Owner]

    H -->|Review Comments| I[Contributor Updates PR]
    I --> E

    H -->|Approved| J[Component Owner<br/>merges to<br/>development branch]

    J --> K[Update & Close<br/>GitHub Issue]
```

## Step-by-Step Explanation

### 1. Contributor Creates a GitHub Issue

The workflow begins when the contributor creates a GitHub issue in the  
`rdk-community-contributions` repository.

This issue is used for:
- Problem statement
- Design discussion
- Proposal evaluation
- Tracking progress and decisions

No code changes are made at this stage.

**Actor:** Contributor<br/>
**Purpose:** Discussion, tracking, and planning

---

### 2. Contributor Forks the Code Repository

After the issue is created, the contributor:
- Forks the relevant code repository
- Implements the required code changes in the fork

**Actor:** Contributor<br/>
**Purpose:** Prepare implementation in an isolated fork

---

### 3. Pull Request Is Raised from the Fork

A Pull Request (PR) is raised from the forked repository to the development branch of the upstream code repository.

Requirements:
- The PR must include a link to the previously created GitHub issue  
  (for example: `Fixes #<issue-number>`)

**Actor:** Contributor<br/>
**Purpose:** Submit code changes for review and validation

**Key Rule:**  
Pull Requests represent implementation, not discussion.

---
## PR Description – Mandatory Fields

To help maintainers and reviewers understand, validate, and review contributions efficiently, **every pull request must include the following fields in its description.**

Please use the exact field names shown below and provide meaningful details for each field.

```markdown
Description:
[Clearly explain what the change does, which component or functionality is affected, and the intended outcome.]

Reason for Change:
[Explain why the change is needed. Include the problem being addressed, the motivation for the change, or the benefit it provides to the RDK stack.]

Test Procedure:
[Describe the tests performed, the steps required to reproduce or validate the change, and the results observed. If testing has not been performed, please state that clearly and explain why.]

Risks:
[Identify potential regressions, compatibility concerns, side effects, or impacts on other components. If no known risks have been identified, state that explicitly.]

Priority:
[Specify the priority and briefly explain its justification.]
```
**A few helpful reminders:**

These five fields are mandatory for every PR, including bug fixes, feature contributions, enhancements, refactoring, and documentation changes.
- Please provide specific information rather than leaving a field blank or using only a generic statement.
- For documentation-only or other non-functional changes, briefly explain why runtime testing is not applicable in the Test Procedure field.
- If additional context, test logs, screenshots, or supporting documents are available, please link them in the PR description.
- Before submitting the PR, check that all five fields are present and complete. This helps reviewers understand the change without unnecessary back-and-forth.
- A complete PR description makes the contribution easier to review and helps maintainers assess its value, test coverage, risk, and readiness for integration.

### 4. Contribution Intake Team Takes Ownership

The Contribution Intake Team reviews the PR for completeness and scope.

The team is responsible for:
- Coordinating builds
- Driving test execution
- Acting as a bridge between the contributor and component owners

**Actor:** Contribution Intake Team<br/>
**Purpose:** Intake, coordination, and validation enablement

---

### 5. Build Creation from Forked PR

Build creation done directly from forked PRs.

Actions:
- Builds are created using the submitted PR code

**Actor:** Contribution Intake Team<br/>
**Purpose:** Validate the contribution in a realistic build environment

---

### 6. Testing and Validation

Required testing is executed based on:
- Component impact
- Change complexity
- Platform requirements

Any issues identified are documented and communicated to the contributor.

**Actor:** Contribution Intake Team<br/>
**Purpose:** Ensure quality, stability, and compatibility

---

### 7. Review by Component Owner

Build artifacts, test results, and PR details are shared with the Component Owner.

The Component Owner reviews:
- Code quality
- Test outcomes
- Design alignment

**Actor:** Component Owner<br/>
**Purpose:** Gatekeeping and technical approval

---

### 8. PR Review Feedback and Iteration

Pull requests may receive review feedback from Component Owners, maintainers, other reviewers, and automated review tools such as GitHub Copilot.

We encourage contributors to review all feedback carefully and use it as an opportunity to improve the quality, security, and maintainability of their contributions.

As a contributor, please:
- Review all comments on your PR, including comments from GitHub Copilot and other automated review tools.
- Evaluate each comment to determine whether it identifies a valid issue, a potential risk, or a useful improvement.
- Address the necessary comments by updating your code, tests, or documentation as appropriate.
- If you believe a comment does not apply to your change or does not require a code modification, provide a brief explanation in the relevant conversation.
- Resolve the corresponding review conversation once the concern has been addressed or an appropriate explanation has been provided, following the repository's review practices.
- Push any required fixes to the same PR branch so that the existing PR is updated.

**Please note:** Contributors are not expected to implement every review suggestion automatically. Some comments may be informational, optional, or not applicable to the proposed change. However, every review comment—including Copilot feedback—should be considered, and necessary issues should be addressed before the contribution is considered ready for approval.If you are unsure whether a suggestion is necessary, discuss it with the reviewer or Component Owner before proceeding.

This cycle continues until all concerns are resolved.

**Actor:** Contributor<br/>
**Purpose:** Iterative improvement

**Rule:**  
Do not open new PRs for fixes.

---

### 9. Merge to Development Branch

Once the PR is approved:
- The PR is merged into the development branch
- The contribution becomes part of the official codebase

**Actor:** Component Owner<br/>
**Purpose:** Final integration

---

### 10. Issue Update and Closure

The original GitHub issue is updated with:
- Merge details
- Final status

The issue is closed once the contribution lifecycle is complete.

**Actor:** Contribution Intake Team<br/>
**Purpose:** Close the tracking loop

---

## Summary of Responsibilities

- **GitHub Issues:** Discussion, tracking, and planning
- **Pull Requests:** Code changes and implementation
- **Contributor:** Code updates and review fixes
- **Contribution Intake Team:** Builds, testing, and coordination
- **Component Owner:** Review, approval, and merge decision

---


## Feature Based Contribution Workflow:

## Technical Rationale for the Feature Based Contribution Model:

The feature-branch-first model introduces a controlled integration layer between external contributions and the main development branch (develop), enabling structured validation and governance.

By routing all contributions through a dedicated feature branch, the workflow ensures that:

- Changes are isolated from active development until they pass review and validation.
- Multi-repository contributions can be coordinated and tested together before promotion.
- CI/CD pipelines, security scans, and compliance checks can be executed without exposing sensitive infrastructure.
- Dependency alignment and integration risks are reduced prior to merging into develop.
- Rollback and recovery are simpler, as incomplete or failing changes remain contained.

---

The following steps outline the agreed contribution workflow once a GitHub Issue is created for feature based contributions.

```mermaid
sequenceDiagram
autonumber

participant C as Contributor
participant CT as Contribution Team
participant CO as Component Owner
participant GR as rdk-community-contributions Repo
participant CCR as Code Contribution Repo(s)
participant FR as Forked Repo(s)

C->>GR: Create contribution issue
GR-->>CT: Notify new issue
CT->>GR: Review issue
CT->>GR: Mark issue as accepted
CT->>CCR: Create feature branch
CCR-->>CT: Feature branch created
CT-->>C: Share feature branch reference

C->>CCR: Request fork of feature branch
CCR-->>FR: Create forked repository
FR-->>C: Fork ready for development

C->>FR: Implement changes in forked feature branch
C->>CCR: Raise PR (Fork → Feature Branch)
CCR-->>CT: Notify PR created
C->>GR: Link PR to contribution issue

CT->>CCR: Review PR
CT-->>C: Share review comments (if any)
C->>FR: Apply fixes (same branch)
FR-->>CCR: Update PR automatically
CT->>CCR: Trigger validation (builds + tests)
CCR-->>CT: Share validation results
CT->>CCR: Merge PR → Feature Branch

CT->>CCR: Raise PR (Feature Branch → Develop)
CCR-->>CO: Notify develop PR ready
CT-->>CO: Share validation summary (L1/L2, unit tests, scans)
CO->>CCR: Review develop PR
CO-->>CCR: Merge PR → Develop
```

---

## Phase 1: Contribution Intake & Approval

**Step 1 – Contributor Creates Contribution Issue**  
Actor: Contributor  
The contributor submits a new contribution issue in the Project Community Repository describing the proposed change.

**Step 2 – System Notifies Contribution Intake Team**  
Actor: Project Community Repo (System)  
The system automatically notifies the Contribution Intake Team about the newly created issue.

**Step 3 – Contribution Intake Team Reviews Issue**  
Actor: Contribution Intake Team  
The team evaluates the issue for:
- Scope alignment
- Completeness
- Compliance
- Relevance to roadmap

**Step 4 – Issue Marked as Accepted**  
Actor: Contribution Intake Team  
If approved, the issue is marked as Accepted, formally allowing development to proceed.

**Step 5 – Feature Branch Creation**  
Actor: Contribution Intake Team  
A dedicated feature branch is created in the Code Contribution Repository.

**Step 6 – System Confirms Branch Creation**  
Actor: Code Contribution Repo (System)  
The repository confirms successful creation of the feature branch.

**Step 7 – Feature Branch Reference Shared**  
Actor: Contribution Intake Team  
The branch reference is shared with the Contributor for development.

---

## Phase 2: Fork Creation & Development

**Step 8 – Contributor Requests Fork**  
Actor: Contributor  
The contributor initiates a fork request of the feature branch.

**Step 9 – Forked Repository Created**  
Actor: Code Contribution Repo (System)  
A forked repository is created under the contributor’s namespace.

**Step 10 – Fork Ready Notification**  
Actor: Forked Repo (System)  
The contributor is notified that the fork is ready for development.

**Step 11 – Development in Fork**  
Actor: Contributor  
The contributor implements required changes in the forked feature branch.

---

## Phase 3: Pull Request to Feature Branch

**Step 12 – Pull Request Raised**  
Actor: Contributor  
A Pull Request (PR) is raised from the forked repository to the feature branch.

**Step 13 – PR Notification to Contribution Intake Team**  
Actor: Code Contribution Repo (System)  
The Contribution Intake Team is automatically notified of the new PR.

**Step 14 – PR Linked to Contribution Issue**  
Actor: Contributor  
The PR is linked to the original contribution issue for traceability.

---

## Phase 4: Review, Iteration & Validation (Feature Branch)

**Step 15 – PR Review Initiated**  
Actor: Contribution Intake Team  
The team reviews the PR for:
- Code quality
- Design adherence
- Security considerations
- Coding standards

**Step 16 – Review Comments Shared (If Any)**  
Actor: Contribution Intake Team  
If changes are required, comments are shared on the PR.

**Step 17 – Contributor Applies Fixes**  
Actor: Contributor  
The contributor updates the same branch in the forked repository addressing review comments.

**Step 18 – PR Automatically Updated**  
Actor: Forked Repo → Code Contribution Repo (System)  
The PR reflects updated commits automatically.

**Step 19 – Validation Pipeline Triggered**  
Actor: Contribution Intake Team  
Build and validation pipelines are triggered (L1/L2 tests, scans, unit tests).

**Step 20 – Validation Results Returned**  
Actor: CI System / Repository (System)  
Validation results are generated and shared with the Contribution Intake Team.

**Step 21 – PR Merged to Feature Branch**  
Actor: Contribution Intake Team  
Upon successful validation and review completion, the PR is merged into the feature branch.

---

## Phase 5: Integration to Develop Branch

**Step 22 – PR Raised (Feature → Develop)**  
Actor: Contribution Intake Team  
A new PR is created from the feature branch to the Develop branch.

**Step 23 – Develop PR Notification**  
Actor: Code Contribution Repo (System)  
The Component Owner is notified of the Develop PR.

**Step 24 – Compliance Summary Shared**  
Actor: Contribution Intake Team  
Validation results and compliance checks are shared with the Component Owner.

**Step 25 – Component Owner Reviews Develop PR**  
Actor: Component Owner  
The Component Owner performs final technical and architectural review.

**Step 26 – Merge to Develop**  
Actor: Component Owner  
Upon approval, the PR is merged into the Develop branch, completing the workflow.

---

## Why This Model Is Used

This model intentionally separates untrusted contributor code from trusted CI execution.

GitHub security restrictions prevent secret-based workflows from running on forked pull requests. By merging reviewed code into internal feature branches first, the project ensures:

- Secure access to CI secrets
- Accurate integration testing
- Compliance with scanning and licensing requirements

---

## Expectations from Contributors

- Provide complete and accurate contribution details in the GitHub Issue or Jira ticket, depending on the entry point chosen.
- Include all five mandatory fields in every PR description: Description, Reason for Change, Test Procedure, Risks, and Priority.
- Follow review feedback constructively, including comments from Component Owners, maintainers, and automated tools such as GitHub Copilot.
- Evaluate all review comments, address necessary issues, and resolve conversations once the concern has been addressed or appropriately explained.
- Submit relevant test cases alongside code
- Keep pull requests focused and scoped to the approved issue/Jira

---

## Expectations from the Contribution Intake Team

- Timely issue triage and communication
- Clear guidance on feature branch details and workflow guidance
- Transparent review feedback and working with Component Owner to get the PRs reviewed and merged.

---

## Exception Workflow:

Review Comments Received on Develop PR (Post-Feature Merge)

```mermaid
sequenceDiagram
autonumber

%% Human Actors
participant C as Contributor
participant CT as Contribution Team
participant CO as Component Owner

%% System Actors
participant CCR as Code Contribution Repo(s)
participant FR as Forked Repo(s)

%% Context
Note over CT,CCR: Initial PR merged into feature branch
CT->>CCR: Raise PR (Feature branch → develop)

%% Review on Develop PR
CO->>CCR: Review develop PR
CO-->>CT: Share review comments

%% Fix Routing (Corrected)
CT-->>C: Request fixes via feature branch
C->>FR: Apply fixes on forked feature branch
C->>CCR: Raise NEW PR (Fork → Feature branch)

%% Feature Branch Update
CT->>CCR: Review & merge fix PR → feature branch

%% Develop PR Auto-Update
Note over CCR: Develop PR updates automatically with new commits
CT-->>CO: Re-share updated develop PR

%% Final Approval
CO->>CCR: Approve & merge PR → develop
```

---

## Roles & Responsibilities

The table below clearly defines ownership at each stage of the contribution lifecycle to avoid ambiguity.

- R = Responsible (performs the work)
- A = Accountable (final decision authority)
- C = Consulted
- I = Informed

| Activity | Contributor | Contribution Intake Team | Component Owner |
|--------|------------|--------------------------|-----------------|
| 1. Create Contribution Issue | R | I | I |
| 2. Review & Accept Issue | I | R / A | C |
| 3. Create Feature Branch | I | R / A | I |
| 4. Share Feature Branch Details | I | R | I |
| 5. Fork Feature Branch | R | I | I |
| 6. Implement Code Changes | R | I | I |
| 7. Raise PR (Fork → Feature) | R | I | I |
| 8. Link PR to Issue | R | I | I |
| 9. Review PR (Feature Branch) | I | R / A | C |
| 10. Provide Review Comments | I | R | C |
| 11. Address Review Comments | R | I | I |
| 12. Validate Builds | I | R / A | I |
| 13. Merge PR to Feature Branch | I | R / A | I |
| 14. Raise PR (Feature → Develop) | I | R | I |
| 15. Share Validation Summary | I | R | I |
| 16. Review Develop PR | I | C | R / A |
| 17. Merge PR to Develop | I | I | R / A |

---

## Closing Notes

This process is designed to balance open collaboration with security, quality, and maintainability. Feedback on the process itself is welcome and encouraged, and improvements will be made as the contribution ecosystem evolves.

Thank you for supporting a healthy and scalable community contribution model.
