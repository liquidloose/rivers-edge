# WordPress Hooks & Filters Reference

A practical reference of the most common and useful actions and filters in WordPress, organized by context.

---

## Table of Contents

- [[#Actions]]
  - [[#Initialization & Loading]]
  - [[#Admin]]
  - [[#Frontend]]
  - [[#Posts & Content]]
  - [[#User & Authentication]]
  - [[#Enqueuing Scripts & Styles]]
  - [[#WP-Cron]]
- [[#Filters]]
  - [[#Content & Formatting]]
  - [[#Query & Posts]]
  - [[#Admin Columns]]
  - [[#REST API]]
  - [[#Miscellaneous]]

---

## Actions

### Initialization & Loading

| Hook | When It Fires | Common Use |
|------|--------------|------------|
| `muplugins_loaded` | After mu-plugins load | Very early bootstrapping |
| `plugins_loaded` | After all plugins load | Plugin compatibility checks, loading textdomains |
| `init` | After WP is fully loaded | Registering CPTs, taxonomies, shortcodes |
| `after_setup_theme` | After theme is loaded | `add_theme_support()`, menus, image sizes |
| `wp_loaded` | After WP, plugins, theme all loaded | Last chance before request handling |
| `template_redirect` | Before template is loaded | Redirects, access control |
| `wp` | After main query is set up | Reading query vars, conditional checks |

```php
add_action( 'init', function() {
    register_post_type( 'product', [ /* args */ ] );
} );
```

---

### Admin

| Hook                    | When It Fires                 | Common Use                                  |
| ----------------------- | ----------------------------- | ------------------------------------------- |
| `admin_init`            | Admin page initializes        | Settings registration, admin-only logic     |
| `admin_menu`            | Admin menu is built           | Adding menu pages, subpages                 |
| `admin_notices`         | Admin notices area            | Displaying admin messages                   |
| `admin_enqueue_scripts` | Admin page loads scripts      | Enqueuing admin-only JS/CSS                 |
| `save_post`             | A post is saved               | Saving custom meta, triggering side effects |
| `save_post_{post_type}` | A specific post type is saved | Same, scoped to one CPT                     |
| `delete_post`           | A post is deleted             | Cleanup of related data                     |
| `add_meta_boxes`        | Meta boxes are registered     | Adding custom meta boxes                    |
| `admin_footer`          | Bottom of admin pages         | Inline JS in admin                          |

```php
add_action( 'save_post_article', function( $post_id ) {
    if ( defined( 'DOING_AUTOSAVE' ) && DOING_AUTOSAVE ) return;
    update_post_meta( $post_id, '_my_field', sanitize_text_field( $_POST['my_field'] ?? '' ) );
} );
```

---

### Frontend

| Hook | When It Fires | Common Use |
|------|--------------|------------|
| `wp_head` | Inside `<head>` | Adding meta tags, inline styles, analytics |
| `wp_footer` | Before `</body>` | Inline JS, tracking pixels |
| `wp_body_open` | After `<body>` tag | Google Tag Manager noscript, skip links |
| `loop_start` | Before The Loop begins | Injecting content before posts |
| `loop_end` | After The Loop ends | Injecting content after posts |
| `get_header` | Before header template loads | Header modifications |
| `get_footer` | Before footer template loads | Footer modifications |

---

### Posts & Content

| Hook | When It Fires | Common Use |
|------|--------------|------------|
| `publish_post` | Post is published | Notifications, cache busting |
| `transition_post_status` | Post status changes | Fires for any status transition |
| `{old_status}_to_{new_status}` | Specific status transition | e.g. `draft_to_publish` |
| `post_updated` | Post is updated | Comparing old vs new values |
| `wp_insert_post` | Any post is inserted/updated | Broad post save hook |
| `before_delete_post` | Before post is permanently deleted | Cleanup before deletion |
| `trashed_post` | Post is moved to trash | Soft-delete cleanup |

```php
add_action( 'transition_post_status', function( $new, $old, $post ) {
    if ( $new === 'publish' && $old !== 'publish' ) {
        // Post just went live
    }
}, 10, 3 );
```

---

### User & Authentication

| Hook               | When It Fires        | Common Use                   |
| ------------------ | -------------------- | ---------------------------- |
| `user_register`    | New user registered  | Welcome emails, default meta |
| `profile_update`   | User profile updated | Syncing data                 |
| `wp_login`         | User logs in         | Login logging, redirects     |
| `wp_logout`        | User logs out        | Session cleanup              |
| `set_current_user` | Current user is set  | Very early user-based logic  |
| `delete_user`      | User is deleted      | Cleanup of user data         |

---

### Enqueuing Scripts & Styles

| Hook                          | When It Fires                 | Common Use                                   |
| ----------------------------- | ----------------------------- | -------------------------------------------- |
| `wp_enqueue_scripts`          | Frontend script/style loading | Enqueuing theme JS/CSS                       |
| `admin_enqueue_scripts`       | Admin script/style loading    | Admin-only assets                            |
| `enqueue_block_editor_assets` | Block editor loads            | Registering block variations, editor scripts |
| `enqueue_block_assets`        | Both editor and frontend      | Styles shared between editor and front       |
| `login_enqueue_scripts`       | Login page loads              | Custom login page styles                     |

```php
add_action( 'wp_enqueue_scripts', function() {
    wp_enqueue_style( 'my-theme', get_stylesheet_uri(), [], wp_get_theme()->get('Version') );
    wp_enqueue_script( 'my-script', get_template_directory_uri() . '/js/main.js', ['jquery'], '1.0', true );
} );
```

---

### WP-Cron

| Hook | When It Fires | Common Use |
|------|--------------|------------|
| `wp_scheduled_delete` | Daily cron | Built-in trash cleanup |
| `{your_custom_hook}` | Your scheduled event | Any recurring background task |

```php
// Register schedule
add_action( 'wp', function() {
    if ( ! wp_next_scheduled( 'my_daily_task' ) ) {
        wp_schedule_event( time(), 'daily', 'my_daily_task' );
    }
} );

// Handle it
add_action( 'my_daily_task', function() {
    // Do work
} );
```

---

## Filters

### Content & Formatting

| Filter                 | What It Filters               | Common Use                                           |
| ---------------------- | ----------------------------- | ---------------------------------------------------- |
| `the_content`          | Post content HTML             | Adding content before/after posts, transforming HTML |
| `the_excerpt`          | Post excerpt                  | Modifying auto-generated excerpts                    |
| `the_title`            | Post title                    | Appending/prepending to titles                       |
| `get_the_excerpt`      | Raw excerpt before display    | Changing excerpt length logic                        |
| `excerpt_length`       | Auto-excerpt word count       | Setting excerpt word limit                           |
| `excerpt_more`         | The `[...]` at end of excerpt | Replacing with a "Read more" link                    |
| `wp_trim_excerpt`      | Trimmed excerpt               | Final control over excerpt output                    |
| `the_permalink`        | Post permalink                | Modifying URLs before output                         |
| `body_class`           | `<body>` CSS classes          | Adding conditional body classes                      |
| `post_class`           | Post wrapper CSS classes      | Adding classes to post elements                      |
| `wp_kses_allowed_html` | Allowed HTML tags             | Expanding what tags `wp_kses` permits                |

```php
add_filter( 'excerpt_length', fn() => 20 );

add_filter( 'excerpt_more', fn() => '&hellip; <a href="' . get_permalink() . '">Read more</a>' );

add_filter( 'the_content', function( $content ) {
    if ( is_singular( 'article' ) ) {
        $content = '<div class="article-wrap">' . $content . '</div>';
    }
    return $content;
} );
```

---

### Query & Posts

| Filter                        | What It Filters                                  | Common Use                                 |
| ----------------------------- | ------------------------------------------------ | ------------------------------------------ |
| `pre_get_posts`               | *(action, not filter)* WP_Query before execution | Modifying main query — use `$query->set()` |
| `query_loop_block_query_vars` | Query Loop block query args                      | Overriding sort/filter for block queries   |
| `found_posts`                 | Total post count for pagination                  | Adjusting pagination math                  |
| `posts_where`                 | SQL WHERE clause                                 | Custom SQL conditions                      |
| `posts_join`                  | SQL JOIN clause                                  | Joining extra tables                       |
| `posts_orderby`               | SQL ORDER BY clause                              | Custom sort logic at SQL level             |
| `posts_clauses`               | All SQL clauses at once                          | Comprehensive query SQL modification       |
| `post_limits`                 | SQL LIMIT clause                                 | Changing how many posts are returned       |
| `get_terms_args`              | Arguments passed to `get_terms()`                | Modifying taxonomy queries                 |
| `terms_clauses`               | SQL for taxonomy queries                         | Custom taxonomy SQL                        |

```php
// Modify Query Loop block for a custom post type
add_filter( 'query_loop_block_query_vars', function( $query, $block ) {
    if ( ( $query['post_type'] ?? '' ) !== 'article' ) return $query;
    $query['meta_key'] = '_article_meeting_date';
    $query['orderby']  = 'meta_value';
    if ( empty( $query['order'] ) ) $query['order'] = 'DESC';
    return $query;
}, 10, 2 );
```

---

### Admin Columns

| Filter | What It Filters | Common Use |
|--------|----------------|------------|
| `manage_{post_type}_posts_columns` | Column headers in post list | Adding/removing/reordering columns |
| `manage_edit-{post_type}_sortable_columns` | Which columns are sortable | Making custom columns sortable |
| `manage_{post_type}_posts_custom_column` | *(action)* Column cell content | Rendering custom column data |

```php
add_filter( 'manage_article_posts_columns', function( $columns ) {
    $columns['meeting_date'] = 'Meeting Date';
    return $columns;
} );

add_action( 'manage_article_posts_custom_column', function( $column, $post_id ) {
    if ( $column === 'meeting_date' ) {
        echo esc_html( get_post_meta( $post_id, '_article_meeting_date', true ) );
    }
}, 10, 2 );
```

---

### REST API

| Filter | What It Filters | Common Use |
|--------|----------------|------------|
| `rest_{post_type}_query` | REST API query args for a CPT | Modifying editor/REST queries |
| `rest_prepare_{post_type}` | REST API response for a CPT | Adding fields to REST responses |
| `rest_pre_insert_{post_type}` | Post data before REST insert | Validating/modifying on create |
| `rest_endpoints` | All registered REST endpoints | Adding or removing endpoints |
| `rest_authentication_errors` | Auth error check | Custom authentication logic |

```php
add_filter( 'rest_article_query', function( $args, $request ) {
    if ( $request->get_param( 'orderby' ) === 'meeting_date' ) {
        $args['meta_key'] = '_article_meeting_date';
        $args['orderby']  = 'meta_value';
    }
    return $args;
}, 10, 2 );
```

---

### Miscellaneous

| Filter                            | What It Filters               | Common Use                               |
| --------------------------------- | ----------------------------- | ---------------------------------------- |
| `upload_mimes`                    | Allowed file upload types     | Enabling SVG or other mime types         |
| `login_redirect`                  | Where user goes after login   | Custom dashboard redirects               |
| `logout_redirect`                 | Where user goes after logout  | Redirecting to homepage                  |
| `cron_schedules`                  | WP-Cron interval options      | Adding custom cron intervals             |
| `wp_mail`                         | Email args before sending     | Modifying from name, headers, body       |
| `wp_mail_from`                    | The From email address        | Changing default WordPress email         |
| `wp_mail_from_name`               | The From name                 | Changing "WordPress" sender name         |
| `allowed_redirect_hosts`          | Safe redirect whitelist       | Allowing external redirect domains       |
| `sanitize_file_name`              | File name on upload           | Cleaning up uploaded file names          |
| `option_{option_name}`            | Any option value on retrieval | Filtering specific options               |
| `pre_update_option_{option_name}` | Option value before saving    | Transforming options before storage      |
| `register_block_type_args`        | Block type registration args  | Adding attributes/context to core blocks |

```php
// Add custom cron interval
add_filter( 'cron_schedules', function( $schedules ) {
    $schedules['every_15_minutes'] = [
        'interval' => 900,
        'display'  => 'Every 15 Minutes',
    ];
    return $schedules;
} );

// Allow SVG uploads
add_filter( 'upload_mimes', function( $mimes ) {
    $mimes['svg'] = 'image/svg+xml';
    return $mimes;
} );
```

---

## Tips

- **Always check `is_admin()`, `is_main_query()`, `doing_action()` etc.** inside hooks to avoid unintended side effects.
- **Return values matter in filters** — always `return` in a filter callback or your data disappears.
- **Priority** defaults to `10`. Lower = earlier, higher = later. Use `PHP_INT_MAX` to run last.
- **`$accepted_args`** — tell `add_filter`/`add_action` how many args your callback needs, or you won't receive them.
- Use `has_action()` / `has_filter()` to check if a hook is registered before removing it with `remove_action()` / `remove_filter()`.

```php
// Correct: 3 args, priority 20
add_action( 'transition_post_status', 'my_callback', 20, 3 );

// Remove a hook added by a class instance
remove_action( 'init', [ $object, 'method_name' ] );
```

---

*Last updated: April 2026*
