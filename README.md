# Renovate configs

Shared dependency-update policy for soupglasses repositories, using the flat,
self-contained preset layout from home-operations/renovate-presets. Each reference
can be pinned to a reviewed commit or release; presets do not reference each other.

```json
{
  "extends": [
    "github>soupglasses/renovate-configs#<reviewed-commit>",
    "github>soupglasses/renovate-configs:providers#<reviewed-commit>"
  ]
}
```

The default policy applies to all repositories. The providers preset is optional
and adds native Crossplane provider managers and grouping. Container image patches
and digest refreshes share a PR, reducing repeated builds for both architectures.
Major image upgrades stay separate. Pin both references
independently. Keep application-specific rules, exceptions, rebase policy, and automerge decisions
in the consuming repository. No preset enables automerge.

The providers preset relies on the native GitHub Actions manager for Mise's
`with.version`; adding a regex manager for that field creates duplicate PRs.

Routine branch creation and existing-branch updates are limited to Mondays in
Europe/Copenhagen. The full-day window accommodates the hosted bot's polling
interval. This is a weekly maintenance window, not exactly one push each week.
GitHub vulnerability alerts remain immediate. Lockfile maintenance follows the
same Monday window when enabled by a repository.

Ops retains its application groups, rollout checks, and explicit review rules.
Urgent non-alert updates can use Renovate's manual rebase controls or a temporary,
reviewed repository schedule override.

Validate changes with the same Renovate version used by the bot:

```sh
renovate-config-validator --strict default.json providers.json
```

Validate both presets before publishing a new signed version tag. Never move a
published tag; repositories adopt a new tag or commit through a reviewed update.

References: https://docs.renovatebot.com/configuration-options/#schedule and
https://docs.renovatebot.com/configuration-options/#updatenotscheduled
