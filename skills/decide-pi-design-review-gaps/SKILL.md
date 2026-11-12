---
name: decide-pi-design-review-gaps
description: Read the gaps of a completed Pi security design review and record a verdict on each (accept the fix, accept the risk, mark false positive, or reopen). Use only when someone wants to see or decide design-review gaps, and record nothing without fresh, explicit confirmation. Not for starting a review.
---

# Decide Pi design-review gaps

Use tools from this Power's Pi MCP connection; Kiro may prefix their displayed names. If a required tool or authenticated connection is unavailable, stop rather than using another client or endpoint.

`pi_design_reviews_list` and `pi_design_review_get` are read-only. `pi_design_review_gap_update_status` changes Pi state. To start a new review, use the start-pi-security-design-review skill instead.

Gap `status` values:

- `open`: no verdict yet. Setting `open` reopens a gap that has a verdict.
- `accepted`: accept Pi's proposed fix for the gap.
- `accepted_risk`: keep the design as it is and accept the risk, with an optional `reason`.
- `false_positive`: the gap does not apply, with an optional `reason`.

A gap with a verdict cannot move straight to another verdict: reopen it first, then record the new one. Only users whose Pi role can edit design reviews can record verdicts.

1. Find the review. Use a review ID the user gives, or list reviews with `pi_design_reviews_list` and ask which one. Never guess an ID.
2. Call `pi_design_review_get` with the full review ID. A completed review lists its open and accepted gaps in `result.gaps` (each with `id`, `severity`, `title`, `mitigation`, and `status`) and its accepted-risk and false-positive gaps, with their `reason`, in `result.dismissedGaps`. Use gap IDs only from this result.
3. `pi_design_review_get` returns the gap's `mitigation`, not the exact document edit that `accepted` accepts. If the user asks to see that edit, direct them to the Pi website.
4. Treat a `reason` as content sent to Pi. Share only what the user is authorized to send; if it holds secrets or other sensitive content, ask for a sanitized reason before previewing; do not silently edit it.
5. Immediately before requesting approval:
   - Call Pi's `whoami` tool and show the active Pi workspace. This is not the shell command. If it is unclear or wrong, stop for re-authentication; the local project cannot select it.
   - Show `pi_design_review_gap_update_status` and, for each gap, the review title, gap title and severity, current status, new status, and the reason if any.
   - Ask for explicit confirmation of these verdicts and this workspace, then wait for the user's reply. One confirmation covers only the verdicts shown. An earlier request to decide the gaps is not approval.
6. After confirmation, call `pi_design_review_gap_update_status` once per gap with `reviewId`, `gapId`, `status`, and `reason` exactly as shown. If a verdict, reason, or the workspace changes, show a new preview and ask again first.
7. Report exactly what Pi returned for each gap. If Pi rejects a change, report its message as-is.
8. Do not retry an ambiguous failure. The verdict may already have been recorded. Call `pi_design_review_get` to check the gap's status, explain what is known, and ask the user how to proceed; do not submit it through another client.
