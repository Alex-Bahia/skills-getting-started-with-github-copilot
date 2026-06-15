# Scaffolding — Plugin, Child Theme, CPT, AJAX

## Code Placement Rules

| Destination | When to use |
|---|---|
| Child theme `functions.php` | Lightweight theme-scoped hooks, no reusability needed |
| Elementor → Site Settings → Custom Code | JS/CSS snippets injected at `wp_head`, `wp_footer`, `wp_body_open` |
| Dedicated plugin `includes/` class | Reusable logic, CPTs, REST endpoints, widget registration |
| `wp-content/mu-plugins/` | Must-load logic, network-wide on multisite, security-critical code |
| Elementor Widget PHP file | Custom `\Elementor\Widget_Base` extension, registered via hook |
| `wp-content/themes/mytheme-child/woocommerce/` | WooCommerce template overrides — **last resort only** |

## Plugin File Structure

```
/wp-content/plugins/myplugin/
├── myplugin.php
├── includes/
│   ├── class-myplugin.php
│   ├── class-myplugin-hooks.php
│   ├── class-myplugin-assets.php
│   ├── class-myplugin-rest.php
│   └── class-myplugin-widget.php
├── assets/
│   ├── css/myplugin.css
│   └── js/myplugin.js
└── README.md
```

## Plugin Main File Header

```php
<?php
/**
 * Plugin Name:  My Plugin
 * Description:  Short description.
 * Version:      1.0.0
 * Requires at least: 6.9
 * Requires PHP: 8.3
 * Requires Plugins: elementor
 * Author:       Your Name
 * License:      GPL-2.0-or-later
 * Text Domain:  myplugin
 */
defined( 'ABSPATH' ) || exit;

define( 'MYPLUGIN_VERSION', '1.0.0' );
define( 'MYPLUGIN_PATH',    plugin_dir_path( __FILE__ ) );
define( 'MYPLUGIN_URL',     plugin_dir_url( __FILE__ ) );

add_action( 'plugins_loaded', function() {
    if ( ! did_action( 'elementor/loaded' ) ) {
        add_action( 'admin_notices', function() {
            echo '<div class="notice notice-warning"><p>'
                . esc_html__( 'My Plugin requires Elementor to be active.', 'myplugin' )
                . '</p></div>';
        } );
        return;
    }
    require MYPLUGIN_PATH . 'includes/class-myplugin.php';
    MyPlugin::instance()->init();
} );

// WooCommerce HPOS compatibility
add_action( 'before_woocommerce_init', function() {
    if ( class_exists( \Automattic\WooCommerce\Utilities\FeaturesUtil::class ) ) {
        \Automattic\WooCommerce\Utilities\FeaturesUtil::declare_compatibility(
            'custom_order_tables', __FILE__, true
        );
    }
} );
```

## Child Theme

```css
/* style.css */
/*
 * Theme Name:   My Theme Child
 * Template:     parent-theme-folder-name
 * Version:      1.0.0
 * Text Domain:  mytheme-child
 */
```

```php
// functions.php
add_action( 'wp_enqueue_scripts', 'mytheme_child_enqueue_styles' );
function mytheme_child_enqueue_styles(): void {
    $parent = 'parent-theme-style';
    wp_enqueue_style( $parent, get_template_directory_uri() . '/style.css', [],
        wp_get_theme( get_template() )->get( 'Version' ) );
    wp_enqueue_style( 'mytheme-child-style', get_stylesheet_uri(), [ $parent ],
        wp_get_theme()->get( 'Version' ) );
}
```

## Custom Post Type

```php
add_action( 'init', 'myplugin_register_post_types' );
function myplugin_register_post_types(): void {
    register_post_type( 'myplugin_item', [
        'labels'         => [ 'name' => 'Items', 'singular_name' => 'Item' ],
        'public'         => true,
        'has_archive'    => true,
        'show_in_rest'   => true, // required for Elementor Loop Grid
        'supports'       => [ 'title', 'editor', 'thumbnail', 'custom-fields' ],
        'menu_icon'      => 'dashicons-portfolio',
        'rewrite'        => [ 'slug' => 'items', 'with_front' => false ],
    ] );
}
// flush_rewrite_rules() only on plugin activation — NEVER on 'init'
```

## AJAX Handler

```php
add_action( 'wp_ajax_myplugin_action',        'myplugin_ajax_handler' );
add_action( 'wp_ajax_nopriv_myplugin_action', 'myplugin_ajax_handler' );

function myplugin_ajax_handler(): void {
    check_ajax_referer( 'myplugin_ajax', 'nonce' );
    if ( ! current_user_can( 'read' ) ) {
        wp_send_json_error( [ 'message' => 'Insufficient permissions.' ], 403 );
        return;
    }
    $item_id = absint( $_POST['item_id'] ?? 0 );
    if ( $item_id <= 0 ) {
        wp_send_json_error( [ 'message' => 'Invalid item ID.' ], 400 );
        return;
    }
    wp_send_json_success( myplugin_get_cached_data( $item_id ) );
}
```
