# orca-portuguese

Official Brazilian Portuguese (pt-BR) language pack for [Orca](https://github.com/stablyai/orca).

## Status

The catalog contains **12,524 strings**, covering settings, sidebars, editor,
terminal, GitHub/GitLab/Linear/Jira integrations, onboarding, mobile companion
app, dashboard, system tray, and application menu.

Missing translations fall back to Orca's English catalog automatically.
Coverage varies with the Orca version as new UI strings are added. Inline CSS
is omitted so the app uses its complete built-in styles.

## Installation

Orca discovers language packs through its plugin system. Point Orca at this
repository (or a local checkout) as a plugin source, then select
**pt-BR — orca-portuguese** from Settings → Appearance → Language.

## How this pack was built

The English source (`en.json` from `stablyai/orca`,
`src/renderer/src/i18n/locales/en.json`) was extracted, split into batches by
UI namespace, and translated with an LLM-assisted, multi-pass process:

1. Flatten the English catalog into `path -> string` pairs, excluding keys
   under the plugin-protected namespace (`auto.components.settings.plugin*`,
   enforced by Orca's own plugin artifact parser) and a handful of entries
   that are actually inline CSS for animated marketing visuals, not
   translatable prose.
2. Translate in batches grouped by component/namespace, with a shared
   style guide (informal `você` register, placeholders preserved verbatim,
   brand names and established Brazilian dev-tooling English loanwords —
   `branch`, `commit`, `worktree`, `workspace`, `pull request`, etc. — kept
   untranslated, consistent with how GitHub/GitLab/VS Code are localized for
   pt-BR).
3. Cross-batch consistency pass: reconciled terminology that drifted between
   independently translated batches (e.g. "Checks" vs "Verificações" vs
   "Checagens" for the GitHub PR checks tab; "Mergeado" vs "Mesclado" for the
   merged-PR/MR status badge — both normalized to match GitHub's own official
   pt-BR localization).
4. Validated against the same rules Orca's plugin loader enforces at runtime
   (`parsePluginLanguagePackArtifact`): max 20,000 entries, max depth 16, no
   dangerous/unsafe keys, no protected paths, no string over 8,192 chars.

Every translated string was reviewed for the rules above; no string was left
identical to English except where that is the correct choice (brand names,
technical loanwords, code/CLI literals, keyboard shortcuts).

## Known limitations

A few upstream i18n design constraints can't be fixed from the translation
side alone — flagging them here for visibility:

- **Positional placeholder pluralization**: some English strings compose a
  sentence from an English verb/noun injected via a positional placeholder
  (e.g. `"{{value0}} PR #{{value1}}?"` where `{{value0}}` is `close`/`reopen`
  in English, or `"session{{value1}}"` for English pluralization). Since
  Portuguese conjugates verbs and pluralizes nouns differently than English,
  these can't be made fully grammatically correct without named,
  language-aware placeholders upstream. The translations preserve the
  placeholders and produce the closest natural phrasing possible.
- **`auto.components.status.bar.WorkspaceSpaceManagerPanel.5c6d25720c`** has
  no pt-BR translation yet; it falls back to English automatically per
  Orca's sparse-catalog policy.
- A small number of ambiguous product-specific terms (e.g. "Conductor",
  "Space" as used in `WorkspaceSpacePage`/`WorkspaceSpaceCompactPanel`) were
  kept in English pending confirmation from the Orca team on whether they are
  intentional feature/brand names or generic words that should be
  translated.

## Contributing

Corrections and improvements welcome — please open a PR against
`locales/pt-BR.json`, keeping the existing key structure and the style
conventions above.
