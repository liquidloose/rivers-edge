---
title: Query Loop Filtered by Views (Custom Field)
tags:
  - gutenberg
  - wordpress
  - blocks
  - query-loop
  - meta-query
source: "[[Query_Loop_Extension_in_Gutenberg]]"
created: 2026-04-11
related:
  - "[[Query_Loop_Extension_in_Gutenberg]]"
  - "[[WordPress_Hooks_and_Filters]]"
processed: true
---

# Query Loop Filtered by Views

Brief summary: How to create a Query Loop block variation that sorts posts by a custom `views` meta field, displaying most-viewed content first.

## Body

This extends the Query Loop block to sort articles by a custom meta field `_article_view_count` (or any `views`-style field). Unlike the default date sort, this surfaces the most-read content first — useful for "Popular Articles" sidebars, homepages, etc.

The implementation has two parts:
1. **JS** — registers the block variation in the editor
2. **PHP** — intercepts the query and applies the meta sort on the frontend

---

### Step 1: Register the Block Variation (JS)

Enqueue this in `enqueue_block_editor_assets`. The key attribute is `__frmCustomFieldFilter` set to `'_article_view_count'` — this is how the PHP side detects which variation is rendering.

```javascript
(function () {
    function tryRegister() {
        if (typeof wp === 'undefined' || !wp.blocks || !wp.blocks.registerBlockVariation) {
            setTimeout(tryRegister, 100);
            return;
        }

        wp.blocks.registerBlockVariation('core/query', {
            name: 'filtered-by-view-count',
            title: 'Query: Sorted by View Count',
            description: 'Displays articles sorted by view count (most viewed first).',
            keywords: ['views', 'popular', 'view count', 'articles'],
            scope: ['inserter', 'block'],
            isDefault: false,
            attributes: {
                __frmCustomFieldFilter: '_article_view_count',
                query: {
                    postType: 'article',
                    perPage: 10,
                    offset: 0,
                    order: 'desc',
                    inherit: false,
                },
            },
            isActive: ['__frmCustomFieldFilter'],
            innerBlocks: [
                ['core/post-template', {}, [
                    ['core/post-title', { isLink: true }],
                ]],
            ],
        });
    }

    tryRegister();
})();
```

---

### Step 2: Filter the Query on the Frontend (PHP)

Hook into `query_loop_block_query_vars` to override the sort.

```php
add_filter( 'query_loop_block_query_vars', function( $query, $block ) {
    if ( ( $query['post_type'] ?? '' ) !== 'article' ) {
        return $query;
    }
    $query['meta_key'] = '_article_view_count';
    $query['orderby']  = 'meta_value_num'; // numeric sort, not string
    if ( empty( $query['order'] ) ) {
        $query['order'] = 'DESC'; // most viewed first
    }
    return $query;
}, 10, 2 );
```

> **Note:** Use `meta_value_num` (not `meta_value`) when the field holds integers. String sort would put `99` after `100` incorrectly.

---

### Step 3: Increment the View Count

```php
add_action( 'wp', function() {
    if ( ! is_singular( 'article' ) ) {
        return;
    }
    $post_id = get_queried_object_id();
    $views   = (int) get_post_meta( $post_id, '_article_view_count', true );
    update_post_meta( $post_id, '_article_view_count', $views + 1 );
} );
```

> **Performance tip:** For high-traffic sites, batch increments via a transient or async request rather than a synchronous `update_post_meta` on every page load.

---

### Step 4: Display the View Count (Optional Block Binding)

```javascript
wp.blocks.registerBlockVariation('core/paragraph', {
    name: 'article-view-count',
    title: 'Article View Count',
    attributes: {
        content: '',
        metadata: {
            bindings: {
                content: {
                    source: 'fr-mirror/article-meta',
                    args: { key: '_article_view_count' },
                },
            },
        },
    },
});
```

---

### How It Differs from the Meeting Date Variation

| | Meeting Date | View Count |
|---|---|---|
| Meta key | `_article_meeting_date` | `_article_view_count` |
| `orderby` | `meta_value` (string/date) | `meta_value_num` (integer) |
| Default order | DESC (newest first) | DESC (most viewed first) |
| Data type | `YYYY-MM-DD` string | Integer |
| Updated by | Editor on publish | `wp` action on page load |

---

### Gotchas

- **Posts with no view count meta** — if `_article_view_count` doesn't exist on a post, add `'meta_compare' => 'EXISTS'` or initialize it to `0` on publish.
- **Caching** — page caches will prevent accurate view counting. Use a JS-based counter or async endpoint for accuracy.
- **Bots** — raw `update_post_meta` counts bots too. Consider checking `is_user_logged_in()` or user agent.

---

## Sources & References
- [[Query_Loop_Extension_in_Gutenberg]]
- [[WordPress_Hooks_and_Filters]]
- https://developer.wordpress.org/reference/hooks/query_loop_block_query_vars/
- https://developer.wordpress.org/reference/classes/wp_query/#order-orderby-parameters
