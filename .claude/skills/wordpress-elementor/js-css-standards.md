# JavaScript & CSS Standards

## JavaScript

- IIFE com ES6+ syntax; evitar jQuery a menos que dependência WP core exija
- **Integração Elementor**: sempre aguardar `elementor/frontend/init` antes de `addAction`
  - Não usar `if(window.elementorFrontend)` — se o objeto já existe, `element_ready` já disparou
  - Scripts que dependem desse evento: `in_footer: true` SEM `strategy: defer`

**Passar dados para JS:**
```php
// wp_add_inline_script() em vez de wp_localize_script()
// wp_localize_script foi projetado apenas para strings i18n
wp_add_inline_script(
    'myplugin-script',
    'const myPluginData = ' . wp_json_encode( [ 'ajaxUrl' => admin_url('admin-ajax.php'), 'nonce' => wp_create_nonce('myplugin_ajax') ] ) . ';',
    'before'
);
```

## Enqueue API (WP 6.3+)

```php
wp_register_script( 'myplugin-script', MYPLUGIN_URL . 'assets/js/myplugin.js', [], MYPLUGIN_VERSION, [
    'strategy'  => 'defer', // NÃO usar em scripts que ouvem elementor/frontend/init
    'in_footer' => true,
] );
```

## CSS Standards (BEM + Tokens)

- Nomenclatura BEM: `.block`, `.block__element`, `.block__element--modifier`
- Design tokens no root do bloco — nunca em `:root` (evita poluição global)
- **NUNCA targete `.elementor-widget-container`** — removido com Optimized Markup (padrão desde 4.0)
- Use a raiz do widget: `.elementor-widget-myplugin-widget` ou classes BEM próprias
- Focus styles: nunca `outline: none` sem substituto — use `outline: 3px solid` com offset
