# React ↔ Elementor — Guia de Integração

Como usar componentes React dentro do Elementor e converter designs React para Elementor.

## Estratégia 1: Web Component (Melhor para Elementor)

Encapsule o componente React como um Web Component para usar diretamente via HTML widget ou widget customizado.

```js
// myplugin/assets/js/my-react-widget.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import { MyComponent } from './MyComponent';

class MyReactWidget extends HTMLElement {
  connectedCallback() {
    const props = {
      title:  this.getAttribute('data-title') || '',
      color:  this.getAttribute('data-color') || '#000',
    };
    const root = ReactDOM.createRoot(this);
    root.render(React.createElement(MyComponent, props));
  }
}

customElements.define('my-react-widget', MyReactWidget);
```

```php
// No widget Elementor — registrar o script e renderizar o custom element
public function render(): void {
    $title = $this->get_settings_for_display('title');
    $color = $this->get_settings_for_display('color');
    echo '<my-react-widget data-title="' . esc_attr($title) . '" data-color="' . esc_attr($color) . '"></my-react-widget>';
}

public function get_script_depends(): array {
    return ['my-react-widget'];
}
```

## Estratégia 2: Tokens CSS Compartilhados

Exportar tokens do design system React como CSS variables e importar no Elementor Variables Manager.

```css
/* tokens exportados do sistema React */
:root {
  --color-primary:    #3B82F6;
  --color-secondary:  #6B7280;
  --color-surface:    #FFFFFF;
  --color-text:       #2A2A2A;
  --space-sm:         8px;
  --space-md:         16px;
  --space-lg:         24px;
  --radius-card:      16px;
  --shadow-card:      0 2px 8px rgba(0,0,0,0.06);
}
```

No Elementor: Site Settings → Variables Manager → colar as variáveis. Depois usar `var(--color-primary)` nos campos de cor do painel.

## Estratégia 3: Converter Design React → JSON Elementor

Use a skill `elementor-studio` para converter um componente React existente em template JSON do Elementor.

**Fluxo:**
1. Forneça o JSX/TSX do componente React
2. A skill analisa os elementos visuais
3. Mapeia cada elemento para o widget Elementor equivalente
4. Gera o JSON importável

**Mapeamento de componentes:**

| React | Elementor |
|---|---|
| `<h1>`, `<h2>` | `heading` widget |
| `<p>`, `<span>` | `text-editor` widget |
| `<button>` | `button` widget |
| `<img>` | `image` widget |
| `<ul><li>` | `icon-list` widget |
| `<div>` card | Inner container |
| `<nav>` | Flex container + `icon-list` |
| `<video>` | `video` widget |
| Badge/pill | `button` (xs, border-radius 100px) |

## Estratégia 4: Elementor + React Híbrido (Avançado)

Para páginas que misturam Elementor e React:

```php
// Enfileirar React apenas nas páginas que precisam
add_action('wp_enqueue_scripts', function() {
    if (!is_page('minha-pagina')) return;

    wp_enqueue_script(
        'react',
        'https://unpkg.com/react@18/umd/react.production.min.js',
        [],
        '18',
        ['in_footer' => true]
    );
    wp_enqueue_script(
        'react-dom',
        'https://unpkg.com/react-dom@18/umd/react-dom.production.min.js',
        ['react'],
        '18',
        ['in_footer' => true]
    );
    wp_enqueue_script('meu-app-react', MYPLUGIN_URL . 'assets/js/app.js', ['react', 'react-dom'], '1.0', ['in_footer' => true]);
});
```

O app React monta em containers com IDs específicos dentro do layout Elementor:
```html
<!-- Colocar no HTML widget do Elementor (apenas script/mount point) -->
<div id="react-app-root"></div>
```

## Estratégia 5: shadcn/ui → Elementor

shadcn/ui usa Tailwind + Radix. Para converter para Elementor:
1. Copiar as CSS variables do `globals.css` do shadcn para o Variables Manager do Elementor
2. Converter cada componente usando a skill `elementor-studio`
3. Para interatividade (dropdowns, dialogs): usar Custom Code do Elementor com o JS vanilla equivalente

## Quando Usar Cada Estratégia

| Cenário | Estratégia |
|---|---|
| Componente React complexo com estado | Web Component (1) |
| Manter visual consistente entre React e Elementor | Tokens compartilhados (2) |
| Converter um design React existente para Elementor | Converter para JSON (3) |
| App React dentro de uma página Elementor | Híbrido (4) |
| Usar shadcn/ui no Elementor | shadcn → Elementor (5) |
