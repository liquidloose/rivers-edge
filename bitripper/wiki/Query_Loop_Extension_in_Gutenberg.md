---
title: Query Loop Extension in Gutenberg
tags:
  - gutenberg
  - wordpress
  - blocks
  - query-loop
source: "[[output/gutenberg-query-research]]"
created: 2026-04-12
related:
  - "[[Gutenberg Blocks Overview]]"
  - "[[WordPress Development]]"
  - "[[Query_Loop_Filter_By_Views]]"
  - "[[WordPress_Hooks_and_Filters]]"
processed: true
---

# Query Loop Extension in Gutenberg

Brief summary: Guide to extending the Gutenberg Query Loop block with custom variations, filters, and patterns for dynamic content in WordPress.

## Body

The Query Loop block in Gutenberg allows displaying dynamic lists of posts, pages, or custom post types. Extending it involves creating custom variations, filters, or patterns to tailor its behavior—e.g., for specific themes, custom queries, or enhanced UI.

This entry covers creating a basic extension via a plugin. Assumes familiarity with WP plugin dev and React.

### Prerequisites
- WordPress 5.8+
- Basic PHP, JS, and React knowledge

### Step 1: Set Up Plugin
Create `query-loop-extension` in `wp-content/plugins/`.

**plugin.php**:
```php
<?php
/**
 * Plugin Name: Query Loop Extension
 * Description: Custom extensions for Gutenberg Query Loop.
 * Version: 1.0
 * Author: Ron Lussier
 */

if ( ! defined( 'ABSPATH' ) ) exit;

function qle_enqueue_assets() {
    wp_enqueue_script(
        'qle-script',
        plugins_url( 'build/index.js', __FILE__ ),
        array( 'wp-blocks', 'wp-element', 'wp-editor' ),
        '1.0',
        true
    );
}
add_action( 'enqueue_block_editor_assets', 'qle_enqueue_assets' );
```

### Step 2: Register Variation
**src/index.js** (build with @wordpress/scripts):
```javascript
import { registerBlockVariation } from '@wordpress/blocks';

registerBlockVariation( 'core/query', {
    name: 'custom-query-loop',
    title: 'Custom Query Loop',
    description: 'Customized for recent posts.',
    icon: 'list-view',
    attributes: {
        query: {
            perPage: 5,
            postType: 'post',
            order: 'desc',
            orderBy: 'date',
        },
    },
    innerBlocks: [
        [ 'core/post-template', {}, [
            [ 'core/post-title' ],
            [ 'core/post-excerpt' ],
        ] ],
        [ 'core/query-pagination' ],
        [ 'core/query-no-results' ],
    ],
} );
```

### Step 3: Add Filters
In **plugin.php**:
```php
add_filter( 'query_loop_block_query_vars', 'qle_custom_query_vars', 10, 2 );

function qle_custom_query_vars( $query, $block ) {
    $query['post__not_in'] = array( 123 ); // Example exclusion
    return $query;
}
```

### Best Practices
- Performance: Use caching for heavy queries.
- Accessibility: Add ARIA labels.
- Test with themes like Twenty Twenty-Four.
- Atomic: Keep extensions modular.

## Sources & References
- [[Gutenberg Docs]]
- [[Query_Loop_Filter_By_Views]]
- [[WordPress_Hooks_and_Filters]]
- https://developer.wordpress.org/block-editor/
