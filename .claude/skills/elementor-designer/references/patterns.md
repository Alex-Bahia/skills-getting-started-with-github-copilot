# Elementor V4 — Layout Patterns

## Hero Full-Height

```
Flexbox (direction: column, min-height: 100vh)
  └── Content (flex-size: grow 1)
      └── ... conteúdo
  └── Rodapé do hero (pina naturalmente no fundo)
```
Usar `Flex Size: grow 1` no conteúdo — nunca alturas fixas.

## Two-Column Layout

```
Flexbox (direction: row, gap: 40px)
  ├── Coluna A (grow: 1, shrink: 1, basis: 50%)
  └── Coluna B (grow: 1, shrink: 1, basis: 50%)
```
Responsivo mobile: mudar direction para `column`.

## Imagem com Frame

```
Container (aspect-ratio: 4/3, overflow: hidden)
  └── Image (width: 100%, height: 100%, object-fit: cover)
```
Deixar o conteúdo ditar a forma do frame — não forçar dimensões fixas.

## Section Header com Número Ghost

```
Flexbox (position: relative)
  ├── Número ghost (opacity: 6%, font-size: 15rem, position: absolute)
  └── Heading (z-index: 1)
```

## Alternância de Background entre Seções

Usar exclusivamente variáveis CSS — nunca cores hardcoded:
```css
/* Variables Manager */
--bg-section-a: #ffffff;
--bg-section-b: #f5f5f5;
```

## Texto Longo com Largura Restrita

```
Flexbox (width: 100%, justify-content: center)
  └── Inner container (max-width: 640px)
      └── Paragraph
```

## Divisor de Seção

Preferir **Div block** ao widget Divider para mais controle de styling:
```
Div (width: 100%, height: 1px, background: var(--color-border))
```

## Principio Base

Todos os estilos locais são marcados com `← LOCAL` para facilitar refatoração futura em classes globais.
