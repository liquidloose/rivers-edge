---
title: FR Mirror — Query Loop custom variations (architecture)
date: 2026-04-14
tags: [wordpress, gutenberg, query-loop, fall-river-mirror, rest-api]
---

## TL;DR

- **Front end:** `query_loop_block_query_vars` reads **block root** attrs (`namespace`, `__frmCustomFieldFilter`).
- **Editor preview:** `core/post-template` spreads **unknown keys from `attributes.query`** into `getEntityRecords` → `/wp/v2/article?…`. Root-only attrs **do not** reliably reach REST.
- **Fix:** set the same markers on the **root** (for `isActive` + PHP block query) **and** duplicate them **inside `query`** (for editor REST per loop).

## Gutenberg mechanism

`post-template/edit.js`: destructures known `query` fields, then `getEntityRecords( 'postType', postType, { …query, …restQueryArgs } )`. Anything extra on `query` becomes REST query args (must be allowed by `rest_{post_type}_collection_params`).

## Theme implementation

| Piece | Role |
|-------|------|
| `js/query-loop-view-count-variation.js` | Variation `fr-mirror/query-view-count`, filter `_article_view_count` |
| `js/query-loop-meeting-date-variation.js` | Variation `fr-mirror/query-meeting-date`, filter `_article_meeting_date` |
| `functions.php` `register_block_type_args` | Persist attrs + context |
| `rest_article_collection_params` | Allow `namespace`, `__frmCustomFieldFilter` (+ optional `filter.properties`) |
| `rest_article_query` | Sort when params present; **no** “empty = legacy meeting date” (that poisons all editor fetches) |
| `query_loop_block_query_vars` | Front-end sort; legacy fallback **OK** here only |

## Anti-patterns

1. Global `wp.apiFetch.use` scanning blocks → **first match wins** → every loop same preview.
2. `rest_article_query` legacy when params missing → **every** REST list sorted by meeting date.
3. Expecting nested `filter[]` alone without `query` embed — still depends on client; **`query` embed is reliable**.

## Ops

- Re-save / re-insert variations so `query` includes markers.
- Unique **`queryId`** per Query block **on the same template**.

## Repo refs

`functions.php`, `js/query-loop-view-count-variation.js`, `js/query-loop-meeting-date-variation.js`
