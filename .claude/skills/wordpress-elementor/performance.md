# Performance & Accessibility Checklists

## Frontend Performance

- Scripts usam `strategy: defer` via WP 6.3+ API — exceto scripts Elementor (`in_footer` sem defer)
- CSS crítico (above-fold) inlining
- `fetchpriority="high"` na imagem LCP
- `loading="lazy"` com dimensões explícitas em imagens below-fold
- WP 6.7+: `auto-sizes` é adicionado automaticamente a imagens lazy — desabilitar via filter se necessário

**WP 6.9+**: suporte a IE conditional comments removido — assets com conditionals serão ignorados se `WP_DEBUG` ativo.

**Speculation Rules API (WP 6.8+)**: prefetch conservador para usuários deslogados.
- Excluir URLs com side-effects via `wp_speculation_rules_href_exclude_paths` ou classe CSS `no-prefetch`

## Backend Performance

```php
// Desabilitar paginação metadata quando não necessário
new WP_Query([
    'no_found_rows' => true, // pula COUNT(*) — ganho significativo em tabelas grandes
    'post_type'     => 'myplugin_item',
]);

// Transients para operações custosas
$key  = 'myplugin_' . md5( $param1 . $param2 ); // md5 garante máx 172 chars
$data = get_transient( $key );
if ( false === $data ) {
    $data = myplugin_expensive_operation();
    set_transient( $key, $data, HOUR_IN_SECONDS );
}
```

- WP 7.0 Real-Time Collaboration usa tabela dedicada — sempre especificar `post_type` explicitamente nas queries
- WP 6.9 reestruturou cache keys de query — plugins que manipulam grupos de cache diretamente devem adotar as novas funções com salt

## Accessibility (WCAG 2.2 AA)

- Elementos interativos: `<button>` ou `<a>` — nunca `<div>` com click handler
- Imagens: `alt` descritivo; decorativas: `alt=""`
- Modais/dialogs: `role="dialog"`, `aria-modal="true"`, focus trap, `inert` via JS
  - `aria-hidden` em dialogs viola especificação ARIA — usar `inert` no restante do DOM
- Navegação completa por teclado obrigatória
- Respeitar `prefers-reduced-motion` em animações
