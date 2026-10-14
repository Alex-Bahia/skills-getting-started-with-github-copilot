# WooCommerce Integration

## HPOS Compatibility (Critical for WC 8.2+)

WC 10.7+ (April 2026): "sync on read" disabled by default — direct writes to legacy tables cause silent data inconsistencies.

**Declaration obrigatória no plugin:**
```php
add_action( 'before_woocommerce_init', function() {
    if ( class_exists( \Automattic\WooCommerce\Utilities\FeaturesUtil::class ) ) {
        \Automattic\WooCommerce\Utilities\FeaturesUtil::declare_compatibility(
            'custom_order_tables', __FILE__, true
        );
    }
} );
```

## Order Data Best Practices

```php
// ✅ HPOS-native
$order = wc_get_order( $order_id );
$value = $order->get_meta( '_my_key' );
$order->update_meta_data( '_my_key', $value );
$order->save();

// ❌ NEVER use with orders
get_post( $order_id );
update_post_meta( $order_id, '_my_key', $value );
// Direct SQL on wp_posts/wp_postmeta for orders
```

## Frontend Customization (ordem de preferência)

1. Elementor Loop Grid patterns com queries customizadas
2. Hook-based via `woocommerce_*` actions/filters
3. Template overrides em `themes/mytheme-child/woocommerce/` — último recurso, manter rastreamento de versão
