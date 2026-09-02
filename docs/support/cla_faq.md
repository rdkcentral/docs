# CLA FAQ

This Frequently Asked Questions (FAQ) guide explains common Contributor License Agreement (CLA) questions for repositories where CLA checks are enabled and/or enforced.

This FAQ focuses on what contributors and maintainers need to do in their own repository.

## Where must the repository workflow file live?

For this current design on GitHub.com, the calling workflow file must be present on the
repository default branch:

- Required file: `.github/workflows/cla.yml`
- Required location/state: active on the current default branch context

You do not need to duplicate `.github/workflows/cla.yml` into every target branch for runtime triggering
behavior.

## How do I sign the CLA?

You sign by following the CLA bot instructions in a pull request comment thread.
There is no external CLA portal.

Recommended flow:

1. Open the PR and wait for the CLA bot's first comment.
2. Check which contributor account(s) the bot says still need to sign.
3. Each identified contributor should add the exact signing phrase shown by the CLA bot from their
    own GitHub account:

    ```text
    I have read the CLA Document and I hereby sign the CLA
    ```

4. Wait for the CLA check to refresh (typically within about 10 minutes).

5. If the signature is still not acknowledged after that wait, add another comment:

    ```text
    recheck
    ```
    This asks the CLA workflow to evaluate again.

6. If rerun controls are available in the PR checks/actions user interface (UI) and you have permission, you can
     also rerun the latest failed CLA check.

Caveat on `recheck` vs manual rerun:

- `recheck` is the most reliable contributor path because it creates a fresh `issue_comment` event.
- Manual rerun availability depends on repository permissions and may not be available to all
    contributors.
- In this implementation, a successful sign/recheck path can also trigger an automatic rerun of
    the latest failed PR-triggered CLA workflow to clear stale failures.

## Where can I find the CLA document itself?

The CLA bot comment links to the current CLA document text. Use that link as the source of truth
for the exact version in effect.

Current configured URL (for cross-reference only):
https://gist.github.com/rdkcmf-jenkins/c797df2d0f276bbae7c2b394e895c263

## Why is my PR blocked with a Required check?

The `Required` label is applied by GitHub rulesets/branch protection policy, not by the CLA
workflow itself.

When a ruleset requires the CLA check context (commonly `Signature / Check`), merge is blocked
until that check reports success.

Workflow trigger conditions are a separate concern: they control whether and when the CLA workflow
run is created, but they do not control whether the check is marked `Required`.

Typical causes of a blocked Required CLA check:

- One or more commit authors in the PR have not signed yet.
- The sign comment text was not exact.
- The latest run has not refreshed after signing.
- Your commit author identity/email does not map to your expected signature/domain policy.

What to do:

1. Read the CLA bot comment and identify the GitHub users it lists as unsigned.
2. Each listed user should sign from their own account using the exact signing phrase.
3. Wait up to about 10 minutes for the CLA check to update.
4. If still pending/failing, add `recheck`.
5. If available and permitted, rerun the latest failed CLA check from the PR checks/actions UI.
6. If still failing, ask a maintainer to inspect the CLA action logs.
7. If you are a maintainer and still need help, raise a CMF support ticket in Jira (`jira.rdkcentral.com`) with pedigree details: repository and PR URL, affected GitHub usernames, latest CLA workflow run URL, exact bot comment/error text, and timestamps of sign/recheck/rerun attempts.

## I already signed before. Why am I being asked again?

Common reasons:

- Another commit author in the PR has not signed yet.
- You are using a different GitHub account than before.
- Email/account mapping differs from what the workflow expects.
- The previous run was stale and needs a `recheck` comment.

## Do I need to sign for every PR?

Normally, no. Once your account is recognized as signed, future PRs should pass CLA automatically.

You may still need to comment `recheck` if a run is stale or if the PR has multiple contributors.

## Is there a way to avoid posting the sign comment if I already signed?

Usually yes, if your account is already recorded and all contributors in the PR are also compliant.
If the check still fails, use `recheck` first.

## Which events trigger the CLA workflow?

In this setup, CLA evaluation runs on:

- PR open/update/close events via pull_request_target.
- New issue/PR comments via issue_comment (filtered by sign/recheck text).

Repositories may additionally restrict target branches. For example,
the workflow can exclude target branches using `branches-ignore` patterns and/or
job-level `if:` guards.

## Why is CLA visible in one repository but not another?

Typical reasons:

- Calling workflow file is missing from the current default branch.
- Branch protection/ruleset does not require the CLA check in that repo/branch.
- Older default-branch cluster has `.github/workflows/cla.yml`, but current default-branch cluster does not.
- Workflow is present, but PR targets branches excluded by filters (if filters were added).

## Our repo references `@v1` in `uses:`. What does that mean for us?

Your repository delegates runtime behavior to the reusable workflow ref:

- `uses: rdkcentral/cmf-actions/.github/workflows/cla.yml@v1`

Implications for repository users:

- A central workflow update to `v1` can change CLA behavior without changing every repository's caller workflow file.
- If behavior changes unexpectedly, maintainers should inspect `rdkcentral/cmf-actions` history/changelog.
- Repository maintainers still control whether check is enforced through local policy (required checks,
  rulesets, branch protection).

## Why do I see CLA check but not as Required?

This means CLA is running but not currently a merge gate on that branch.

Possible states:

- Informational only: check reports status, merge can proceed.
- Partially enforced: required on some branches, not others.
- Recently changed policy: check exists but ruleset update is pending.

Ask maintainers which branch policies are intended for that repo.

## Does CLA work for pull requests from forks?

Yes. The CLA workflow is designed to handle fork PRs by running in the base repository context.

Practical effect:

- Fork contributors can sign by commenting on the PR in the base repo.
- Maintainers still see CLA check results in the base repo PR checks.

## I see “Required” on a CLA status check. Does that mean CLA is enforced?

Usually yes. “Required” indicates branch protection/ruleset policy is gating merge on that check.

Contributors may not see full policy internals, but they will see:

- Required check status in PR checks
- Pull request feedback with a tally of signature status per commit author in the PR
- Merge blocked until required checks pass

In current implementations, the CLA check context often appears as `Signature / Check`.

## What about direct pushes to protected branches?

If branch protection or a ruleset enforces restrictions, direct pushes may:

- Be blocked entirely for most users, or
- Be allowed only through explicitly authorized bypass paths (for example, approved teams, roles,
  or apps).

Recommended practice:

- Prefer PR-based flow for code changes.
- Keep CLA as a required PR check on protected branches.

## Why can protected-branch pushes still happen in some cases?

CLA in this setup is primarily a PR-time check. If a protected branch allows approved bypass
entities (such as specific teams, roles, or apps), those paths may avoid PR check gates by design.

This is a repository-policy decision, not a CLA action failure.

## Why did another continuous integration (CI) job not start when CLA failed?

Some pipelines are configured so downstream checks wait on required gates. If CLA fails, other scans/tests may not start or may be considered blocked.

Resolve CLA first, then rerun or requeue other checks.

## What if the CLA check appears stuck or wrong?

Try this sequence:

1. Confirm your sign comment is exact.
2. Wait about 10 to 15 minutes for status refresh.
3. Add `recheck` if status still appears stale.
4. If available and permitted, rerun the latest failed CLA check from the PR checks/actions UI.
5. Verify the latest CLA workflow run in Actions.
6. Confirm the PR includes no unsigned co-authors/contributors.
7. Ask a maintainer to review workflow logs if still unresolved.

## Do bots need to sign?

Some bots are allowlisted (for example, dependabot-style accounts), so they may be exempt. This is controlled by workflow allowlist settings.

## Can maintainers waive CLA failures?

Depends on repo policy:

- If CLA check is marked Required and no bypass is granted, merge stays blocked.
- If repository policy grants approved bypass entities under rulesets/protection, governance may allow an override.

Project teams should define and document when bypass is acceptable.

## Is CLA needed for first-time external contributors?

Yes, for repositories where CLA enforcement is enabled. First contribution is typically when the contributor is prompted to sign in the PR comments.

## Quick Contributor Checklist

1. Open PR.
2. If prompted, comment: I have read the CLA Document and I hereby sign the CLA
3. Wait about 10 to 15 minutes for the CLA check to refresh.
4. If still pending/failing, comment: recheck
5. If available and permitted, rerun the latest failed CLA check from PR checks/actions.
6. If still blocked, ask maintainer to inspect CLA action logs.

## Quick Maintainer Checklist

1. Confirm CLA check is present and Required where intended.
2. If contributor is blocked, ask for exact sign phrase, then wait-first, then `recheck`.
3. Inspect Actions logs when status looks inconsistent.
4. Confirm ruleset/branch protection behavior for PR checks and approved bypass entities.
5. Avoid merging around CLA unless policy explicitly permits.

## Repository Triage Matrix

Use this when users report "CLA seems off" in a repository:

| What user reports | Likely cause | First maintainer check |
|---|---|---|
| No CLA check appears on PR | Calling workflow not active from current default branch context | Confirm `.github/workflows/cla.yml` exists on current default branch |
| No CLA check on a specific target branch | Branch filters exclude that branch family | Check `branches-ignore` and job-level `if:` guards in `.github/workflows/cla.yml` |
| CLA check appears but not Required | Policy not gating merge on that branch | Inspect branch protection/ruleset required checks |
| CLA check fails even after signing | Exact phrase mismatch, stale run, or unsigned co-author | Ask for exact sign comment + `recheck`, then inspect logs |
| Some repos enforce, others do not | Repo-level policy/config drift | Compare caller workflow presence and required-check policy |
| Worked before, now missing after branch migration | Default branch changed and workflow/policy did not follow | Verify workflow and required checks on new default branch |
