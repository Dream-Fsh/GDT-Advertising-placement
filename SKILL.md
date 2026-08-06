---
name: gdt-advertising-placement
description: Build or review Tencent GDT campaigns in Chuangliang using the approved batch-creation SOP. Use for importing advertiser accounts, configuring products, placements, conversion, bidding, targeting, creatives, materials, copy, landing pages, preview validation, or automation of this workflow.
---

# GDT Advertising Placement

Use this skill to build Tencent GDT ads in Chuangliang's batch-create flow. Keep the detailed SOP local; use the workflow and guardrails below as the reusable execution contract.

## Operating Boundary

- Stop after generating and validating the campaign preview.
- Request explicit user confirmation immediately before the platform's final submit action.
- Treat account IDs, products, brand images, landing pages, copy, bid range, and budget as inputs. Do not reuse values from an earlier campaign unless the user provides them again.

## Required Inputs

Collect or derive these items before starting:

1. Advertiser names, account IDs, and expected account count.
2. Product search keyword or product name.
3. Brand-image keyword based on advertiser name.
4. Targeting template.
5. Material directory, date range, filter metrics, and thresholds.
6. Exact ad copy.
7. Account-to-landing-page mapping.
8. Bid range and budget.

If a required resource varies by account, do not use a shared configuration. Configure each account individually and report any unavailable resource.

## Workflow

1. Import account IDs and select the matching account rows.
2. Create the campaign, select the optimization goal before choosing the product, and validate any shared-product source-account prompt.
3. Configure placements, conversion, bidding, schedule, naming, and targeting.
4. Configure the component creative, brand image, button copy, materials, ad copy, and landing page.
5. Generate the preview and validate every account before requesting confirmation for final submission.

## Non-Negotiable Defaults

- Use smart placements with the approved stable-exploration placement set.
- Use API reporting, form-appointment conversion, and click attribution.
- Use the approved targeting template and selected brand-image rule.
- For materials, choose average allocation for both account and creative-group allocation; set creative-group material limit to `1`.
- Add images from the global bulk-add menu, not an individual creative group.
- For copy, use all-account reuse, enable multi-copy testing, and select exactly two approved copy items.

## Browser Automation Rules

1. Wait for visible state changes after each selection, including selected counts, success notices, saved values, or enabled next-step controls.
2. Check product pages from the last page backward when the product is not immediately available.
3. Verify checkbox state and selected-count panels before confirming modals; search results alone are not a selection.
4. Treat shared-resource dialogs, material-risk dialogs, and preview generation as explicit checkpoints.
5. On mismatched account, product, brand image, landing page, permission, or allowlist state, stop and return an exception list instead of guessing.

## Completion Criteria

Report the following before asking for final submission approval:

- Selected accounts and count.
- Product, placement, conversion, bid, targeting, brand image, materials, copy, and landing page validation results.
- Preview status and any skipped or exceptional accounts.
- Exact external action that final submission will create.
