# Next.js Multilingual and SEO Rules

These rules apply to every AI agent and engineer implementing or changing Arabic/English frontend content, routing, navigation, or metadata. Inspect the existing application first; examples in an implementation brief are not permission to replace its architecture.

## Scope and rendering

1. Support `ar` and `en`, with Arabic as the default. Indexable public pages must have distinct, always-prefixed URLs: `/ar`, `/en`, and equivalent nested routes. Explicit locale URLs remain stable regardless of browser language or cookies. Document whether unprefixed requests use detection or deterministic Arabic redirects.
2. Public marketing and other indexable content must render translated headings and main copy in the initial server HTML. Use Server Components and server-safe translation APIs. Keep interactive Client Component boundaries narrow; do not convert whole pages to Client Components for translations.
3. SSG and ISR satisfy crawlability. Preserve each page's appropriate caching and data-loading strategy; do not force request-time SSR across the site.
4. The authenticated application's client-rendered model, React Query, API ownership, session handling, and Business Time Zone remain in effect. Public-page server rendering is a scoped exception to older statements that every page is a Client Component; it does not introduce a BFF or migrate private application data fetching.

## Audit before editing

5. Establish exact installed Next.js, React and i18n versions, package manager, App/Pages Router structure, root layouts, route groups, config/plugins, proxy/middleware, providers, fonts, styling and existing localization. Read the installed Next.js guides when available and version-matched official i18n documentation before choosing APIs.
6. Inventory real public/static/dynamic routes, translated content availability, protected routes, API/auth/payment endpoints, callbacks, redirects, assets, metadata, structured data, sitemap, robots, environment configuration and deployment origin. Report a compact plan and touched paths before implementation. Do not create illustrative pages or fake catalog entries.
7. Preserve existing behavior and uncommitted work. Compose with existing proxy/auth/rewrites rather than overwriting them. Report Pages Router use before any structural migration. Discuss a material production-route migration before proceeding unless already explicitly authorized.

## One localization system

8. Prefer `next-intl` for Next.js App Router. Reuse an existing compatible system or migrate it deliberately; never add a competing second localization system. Install with the repository's package manager and record the resolved version.
9. Keep routing, navigation and request configuration together behind the cross-cutting `shared/i18n` boundary. Expose locale-aware navigation through `createNavigation` and a public adapter. Keep all application path definitions in `shared/routes/ROUTES`; locale-aware helpers consume this registry. Message namespaces follow features and existing catalogue conventions.
10. For Next.js **16.3+**, prefer the documented `next/root-params` integration when supported by the installed `next-intl`. The locale segment must be at or above its root layout; generated getter names follow segment names. Generate types with Next.js tooling. Root-param getters are for Server Components, not Client Components, Route Handlers or Server Actions; pass/resolve and validate the locale explicitly for those contexts.
11. For Next.js **16.0–16.2**, use the supported `requestLocale` + `setRequestLocale` integration unless `experimental.rootParams` is explicitly enabled and supported. Validate the locale and call `setRequestLocale` before translation APIs in every relevant static page/layout. Do not combine the legacy and root-params approaches or assume a newer API exists.
12. Use `generateStaticParams` for known locale variants where appropriate, including the requirements of Cache Components. Reject invalid locales and absent content variants through real not-found or documented fallback behavior.
13. Use the Next.js 16 `proxy.ts` convention. Intentionally exclude API/auth callbacks, framework internals and static assets; account explicitly for valid public slugs containing dots. Preserve deployment settings and existing config wrappers when adding the `next-intl` plugin.
14. Locale-scoped layouts own one `<html>` root with `lang="ar" dir="rtl"` or `lang="en" dir="ltr"`. Preserve CSS, fonts and global providers. Use the client provider for interactive translated subtrees, passing only the needed messages when practical.

## Content, navigation and direction

15. Move real UI copy, accessible names, alt text, errors and metadata into Arabic/English messages. Use safe rich-text interpolation and locale-aware numbers, dates and currencies. Keep identifiers, route parameters, API field names, SKUs and prices stable. UI locale must not change scheduling instants or the Business Time Zone.
16. CMS/catalog content stays in locale-aware records or translation fields. Handle missing variants intentionally; never serve a full English page under `/ar` while presenting it as translated. Do not invent approved translations for legal, medical or unknown business claims. Preserve exact approved content, mark an original-language excerpt accurately, and report translation approval gaps.
17. Keep internal navigation in the active locale using the shared navigation adapter and `ROUTES`. Provide a visible keyboard-accessible language selector labelled `العربية` / `English`. Switch to the equivalent current page, preserving dynamic parameters and relevant query/hash state; unavailable translations need a clear fallback, never a broken alternate.
18. Use logical start/end styling, Arabic-capable fonts and correct directional icons. Preserve intentional graph/diagram coordinates and isolate Latin identifiers or currency where required. Verify desktop/mobile navigation, forms and interactive states in RTL and LTR.
19. Essential content must remain visible with JavaScript disabled. Scroll/reveal animations progressively enhance visible server content; they must not leave headings or copy permanently hidden without hydration.

## SEO contract

20. Every indexable page type has localized title/description and appropriate Open Graph/social metadata. Each language variant has a self-referencing, page-specific canonical; never canonicalize all routes to the homepage or default language.
21. Centralize the absolute deployment origin in validated configuration such as `NEXT_PUBLIC_SITE_URL`. Normalize slashes and encode dynamic segments in a shared route-aware URL helper. Do not fabricate a production domain or publish localhost/preview canonicals as production. If the origin is unset, omit absolute SEO URLs and report deployment configuration as outstanding.
22. Choose one consistent `hreflang` strategy: verified HTTP Link headers, HTML `alternates.languages`, or sitemap annotations. All advertised URLs must be absolute, reciprocal, and point to real equivalent pages, including the current locale. Disable automatic `next-intl` alternate headers when manual metadata would conflict. Use localized pathname mappings for different slugs and advertise only available variants. Add `x-default` only for a real fallback.
23. A sitemap enumerates all and only real indexable localized pages, including published dynamic records from the real content source, with corresponding alternates. Do not list dashboards, login pages, redirects, unavailable translations or placeholder products. Use actual modification timestamps only. If the origin is absent, return no fabricated entries and report that configuration gap.
24. Keep valid `robots.txt` with the configured sitemap origin. Ensure locale routes are not accidentally blocked or `noindex`; robots directives do not replace access control. Preserve existing structured data, breadcrumbs, pagination and localized internal links.

## Required verification and handoff

Before claiming implementation complete:

- Verify `/` redirect behavior; `/ar` and `/en`; representative real nested/dynamic routes if present; invalid locales and missing variants; query/parameter-preserving language switching; and unaffected API/auth/forms/assets.
- Inspect HTTP responses without client JavaScript: translated main headings/copy, visible content, `lang`/`dir`, localized metadata, self-canonicals, reciprocal alternates, redirect headers/statuses, valid sitemap/robots and absence of conflicting locale signals.
- Inspect RTL/LTR at narrow and wide viewports and exercise affected interactions. If browser verification is unavailable, say so.
- Run existing lint, typecheck, tests and production build as applicable. Fix introduced failures and distinguish pre-existing failures, unavailable commands and skipped checks. Verify that static/dynamic route output matches the intended rendering strategy.
- Report the installed versions/API branch, changed files, example URLs, SEO strategy, checks actually run, remaining untranslated/approval-dependent content and missing production configuration. Leave changes uncommitted unless delivery was requested.

For documentation-only changes, validate links and the diff; do not claim application or browser checks were performed.

## Official references

Use the installed versions' APIs; these references must be rechecked when implementing or upgrading:

- [Next.js internationalization](https://nextjs.org/docs/app/guides/internationalization)
- [Next.js root params](https://nextjs.org/docs/app/api-reference/functions/next-root-params)
- [next-intl routing setup](https://next-intl.dev/docs/routing/setup)
- [next-intl routing configuration](https://next-intl.dev/docs/routing/configuration)
- [next-intl navigation](https://next-intl.dev/docs/routing/navigation)
- [next-intl metadata and Route Handlers](https://next-intl.dev/docs/environments/actions-metadata-route-handlers)
- [Next.js metadata](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)
- [Next.js sitemap](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap)
- [Google localized versions and hreflang](https://developers.google.com/search/docs/specialty/international/localized-versions)
