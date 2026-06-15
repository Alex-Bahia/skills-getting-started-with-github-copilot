# Off-Canvas Filter UI Pattern

## Quando usar este padrão

Use em vez do widget Off-Canvas nativo do Elementor quando precisar de:
- Painel renderizado em `wp_body_open` (fora de ancestrais Elementor com transform)
- UI de filtros conectada a Loop Grid ou queries customizadas
- Controle total sobre `inert` e focus-trap
- Comportamentos que o widget nativo não oferece

## Estrutura HTML

```html
<!-- via wp_body_open hook — filho direto do body -->
<div id="myplugin-offcanvas"
     role="dialog"
     aria-modal="true"
     aria-labelledby="myplugin-offcanvas-title"
     class="myplugin-offcanvas">
  <div class="myplugin-offcanvas__inner">
    <button class="myplugin-offcanvas__close" aria-label="Fechar filtros">×</button>
    <h2 id="myplugin-offcanvas-title">Filtros</h2>
    <!-- conteúdo do filtro -->
  </div>
</div>
<div class="myplugin-offcanvas__backdrop" aria-hidden="true"></div>
```

## CSS Essencial

```css
.myplugin-offcanvas {
  position: fixed;
  inset-block: 0;
  inset-inline-start: 0;
  width: min(400px, 90vw);
  height: 100dvh; /* dvh para mobile Safari */
  z-index: 9999;
  visibility: hidden;
  pointer-events: none;
  transform: translateX(-100%);
  transition: transform 0.3s ease, visibility 0.3s; /* transition no elemento base, não no modifier */
}

.myplugin-offcanvas.is-open {
  visibility: visible;
  pointer-events: auto;
  transform: translateX(0);
}

@media (prefers-reduced-motion: reduce) {
  .myplugin-offcanvas { transition: none; }
}
```

## JavaScript

```js
const panel    = document.getElementById('myplugin-offcanvas');
const backdrop = document.querySelector('.myplugin-offcanvas__backdrop');
const mainContent = document.getElementById('main'); // ou wrapper principal

function openPanel() {
  panel.classList.add('is-open');
  mainContent.inert = true; // foco fica preso no painel
  // iOS Safari: preservar scroll position
  document.body.style.top = `-${window.scrollY}px`;
  document.body.style.overflow = 'hidden';
  panel.querySelector('.myplugin-offcanvas__close').focus();
}

function closePanel() {
  panel.classList.remove('is-open');
  mainContent.inert = false;
  const scrollY = parseInt(document.body.style.top || '0') * -1;
  document.body.style.overflow = '';
  document.body.style.top = '';
  window.scrollTo(0, scrollY);
}

backdrop.addEventListener('click', closePanel);
document.addEventListener('keydown', (e) => { if (e.key === 'Escape') closePanel(); });
```
