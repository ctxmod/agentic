# Lint-vs-Behavior Pre-Flight

Before committing to a UI pattern a linter may flag, probe the rule's configurability at the pinned version; lint fixes may restructure but never remove behavior; the only sanctioned escape is an authorized scoped override with a naming comment.

Test: accessibility targets and error states are not lint findings, and no fix reaches zero findings through a fake interactive role, a tabIndex workaround, or a silent suppression.

The full rule, the worked conflict, and the escape ladder: [lint_behavior_preflight_detail.md](../reference/lint_behavior_preflight_detail.md).
