# REST API Custom Endpoints

## Registration

```php
// sempre em rest_api_init — nunca em functions.php diretamente
add_action( 'rest_api_init', function() {
    register_rest_route( 'myplugin/v1', '/items/(?P<id>\d+)', [
        'methods'             => WP_REST_Server::READABLE,
        'callback'            => 'myplugin_get_item',
        'permission_callback' => 'myplugin_rest_permissions',
        'args'                => [
            'id' => [
                'type'              => 'integer',
                'minimum'           => 1,
                'sanitize_callback' => 'absint',
                'validate_callback' => 'rest_validate_request_arg',
                'required'          => true,
            ],
        ],
        'schema' => 'myplugin_rest_schema', // obrigatório para discovery
    ] );
} );
```

## Permission Callback

```php
// retornar WP_Error (não false) para mensagem JSON adequada
function myplugin_rest_permissions( WP_REST_Request $request ): bool|WP_Error {
    if ( ! current_user_can( 'read' ) ) {
        return new WP_Error( 'rest_forbidden', 'Insufficient permissions.', [ 'status' => 403 ] );
    }
    return true;
}
```

## Autenticação via Nonce

```js
// nonce REST é diferente do nonce AJAX
wp_create_nonce( 'wp_rest' ) // PHP
// enviar como header: X-WP-Nonce
// nonces expiram em 24h — implementar refresh via wp_ajax_rest_nonce
```

## Atenção ao Schema

- WordPress REST usa **JSON Schema draft-04** — não draft-07
- Keywords draft-07 como `if/then/else` ou boolean `exclusiveMinimum` serão ignoradas silenciosamente
