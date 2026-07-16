# locflow-demo

A demo repo for exercising the **locflow** ongoing GitHub App. It contains a
realistic i18next catalog of ~100 English UI strings. Change an English string
in a PR and the app should translate the changed keys into `fr`, `de`, `es`,
and `ja`, commit them back to the PR branch, and post a Check Run.

## Layout

```
.locflow.yml                     # app config (source/target locales, catalog path)
locales/en/translation.json      # source strings (English) — the app diffs THIS
locales/fr|de|es|ja/translation.json   # target catalogs (seeded empty; app fills them)
```

## What's in the strings (so you can test each behavior)

- **Plain strings** — most keys (`common.*`, `nav.*`, `errors.*`).
- **Interpolations** — `{{name}}`, `{{count}}`, `{{amount}}`, etc. The app must
  preserve these exactly (placeholder-safety check).
- **Plurals** — i18next `_one` / `_other` pairs (`notifications.newMessages_*`,
  `projects.membersCount_*`).
- **HTML / Trans tags** — `onboarding.welcomeHtml`, `legal.termsHtml` use
  `<1>…</1>` markers that must survive translation.
- **Do-not-translate** — `app.brandName` ("Locflow") and
  `legal.companyLegalName` ("Locflow, Inc.") are listed in `.locflow.yml` and
  should be left untouched.

## How to trigger the app

1. Make sure the locflow App is **installed on this repo** and its webhook URL
   points at `https://locflow-ghapp.fly.dev/api/github/webhooks`.
2. Create a branch and edit an English value, e.g. in
   `locales/en/translation.json` change `common.save` from `"Save"` to
   `"Save changes"`, or add a brand-new key.
3. Open a pull request against `main`.
4. Watch the PR: the app translates the changed/added keys, commits the updated
   `fr`/`de`/`es`/`ja` catalogs to your branch, and posts a **Check Run** summary.

### Things worth trying
- Change **one** key → confirm only that key is translated (not all 100).
- Edit a string with a `{{placeholder}}` → confirm the placeholder is preserved.
- Change `app.brandName` → confirm it is **not** translated (do-not-translate).
