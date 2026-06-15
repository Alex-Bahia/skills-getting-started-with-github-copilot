# PHP Standards for WordPress Development

## Security Principles

**Input Sanitization:**
- Always `wp_unslash()` before sanitizing `$_POST`/`$_GET` (WP magic-quotes all superglobals)
- `absint()` and `sanitize_key()` handle slashes automatically — no need to unslash first
- Use `sanitize_text_field()`, `sanitize_email()`, `sanitize_url()`, `absint()` per data type

**Output Escaping (at point of output):**
- `esc_html()` — plain text
- `esc_attr()` — HTML attributes
- `esc_url()` — URLs
- `wp_kses_post()` — trusted HTML (post content)

**Nonces:**
- Every form and AJAX action requires a nonce
- Use `sanitize_key()` when verifying (strips slashes implicitly)
- AJAX: use `check_ajax_referer()` — not `wp_verify_nonce()` — idiomatic, fires logging action, dies on failure

## Error Handling

- Return `WP_Error` objects for communicable failures, never throw exceptions
- Always check `is_wp_error()` before processing API responses
- Guard `error_log()` with `WP_DEBUG` checks

## Caching

- Transient keys: 172-character max — use `md5()` for dynamic keys combining multiple values

## Modern PHP Practices

- `declare(strict_types=1)` immediately after opening `<?php`
- Readonly classes (PHP 8.2+) for immutable config objects
- Typed class constants (PHP 8.3+) instead of `define()`
- Prefix ALL global symbols (functions, classes, hooks, options, CSS)

## Password Hashing (WP 6.8+)

- WP now uses bcrypt (`$wp$2y$` prefix) and BLAKE2b for application passwords
- Always: `wp_check_password()` to verify, `wp_hash_password()` to hash
- `wp_fast_hash()` — only with high-entropy random input (>128 bits), never passwords
