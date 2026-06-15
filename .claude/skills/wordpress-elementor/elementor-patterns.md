# Elementor-Specific Patterns (v4.x)

## V4 Compatibility Requirements

- **Never target `.elementor-widget-container`** — this wrapper doesn't exist in V4 when Optimized Markup is enabled (default since 4.0)
- **Include `has_widget_inner_wrapper(): false`** on all new widgets
- **Avoid `strategy: 'defer'`** on scripts listening for `elementor/frontend/init`
- **Use V3 `Widget_Base`** for third-party widgets; V4 Atomic Element PHP API docs remain incomplete

## Mandatory Control-Based Styling

ALL visual properties must be Elementor controls using `selectors` to inject CSS — never hardcode colors, fonts, sizes, or spacing.

Every widget requires:
- `Group_Control_Typography` for text elements
- `Group_Control_Background`, `Group_Control_Border`, `Group_Control_Box_Shadow` for containers
- Responsive controls via `add_responsive_control()` for spacing
- Separate `:hover` sections for interactive states

## Required Widget Methods

```php
// ✅ REQUIRED on EVERY widget
public function has_widget_inner_wrapper(): bool {
    return false;
}

// ✅ REQUIRED on EVERY widget
protected function is_dynamic_content(): bool {
    return false; // true only if output varies per user/session/time
}
```

## Dynamic Tags

Category constants: TEXT, URL, IMAGE, COLOR, etc.
⚠️ POST_GROUP and SITE_GROUP cause fatal errors without Elementor Pro.

## Loop Grid Queries

```php
add_filter( 'elementor/query/{query_id}', function( $query ) {
    $query->set( 'no_found_rows', true ); // performance: skip COUNT query
    $query->set( 'post_type', 'myplugin_item' );
} );
```

## Critical Warnings

- Use `{{WRAPPER}}` in selectors — resolves to widget root class
- Omit type hints for Elementor Pro parameters (Fatal TypeError risk)
- Register dynamic tag groups before tags that reference them
- Use `sanitize_text_field()`, not `sanitize_key()`, for ACF field names (preserves case)
