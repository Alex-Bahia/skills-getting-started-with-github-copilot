# AK Elementor Studio — Design to Elementor Converter

Converte qualquer input de design (HTML/CSS, screenshots, imagens, PDFs, Figma exports, wireframes) em templates JSON prontos para importar no Elementor.

## Regras Absolutas

### Regra #1: HTML Widget = Apenas JavaScript
O widget HTML tem exatamente um uso válido: embedir tags `<script>`. NUNCA para headings, textos, badges, navs, cards, layouts, pricing, timelines, ícones, stats ou QUALQUER conteúdo visual. Todo elemento visual tem um widget nativo Elementor.

### Regra #2: JSON Sem Erros
- Sem comentários (`/* */` ou `//`)
- Sem vírgulas finais
- IDs únicos de 8 caracteres hexadecimais
- Todo elemento requer `"elements": []`
- `isInner: false` para filhos diretos de seção; `isInner: true` para containers aninhados
- Cores apenas em hex — nunca nomes de cores

## Entrevista Pré-Build (Obrigatória)

Antes de construir, coletar:
1. **Sitemap** — Número de páginas
2. **Versão Elementor** — Free ou Pro (Pro habilita Theme Builder, CSS customizado, sticky)
3. **Custom Post Types** — Precisa de ACF field groups?
4. **Formato de saída** — .json individual, ZIP nativo, ZIP Envato, ou misto
5. **Brand Guidelines** — Cores hex + Google Fonts (ou extrair do design)

## Pipelines de Input

| Input | Pipeline |
|-------|----------|
| HTML/CSS/JS | Code Pipeline |
| Imagens/screenshots/mockups | Visual Pipeline |
| PDF | PDF Pipeline |
| Descrição em texto | Description Pipeline |
| JSON existente | Edit Pipeline |
| Misto | Ambos, mesclados |

## Widgets Nativos por Categoria

**Texto & Números:** `heading`, `text-editor`, `counter`, `progress`
**Ações:** `button`, `accordion`, `tabs`, `alert`
**Mídia:** `image`, `video`, `image-gallery`, `image-carousel`
**Cards & Listas:** `icon-box`, `icon-list`, `image-box`, `testimonial`
**Layout:** `divider`, `spacer`
**Utilitários:** `social-icons`, `google-maps`, `shortcode`

## Elementos Complexos SEM HTML Widget

| Elemento | Implementação Nativa |
|---|---|
| Navbar | Flex container: `image` (logo) + `icon-list` (links) + `button` (CTA) |
| Badge/pill | `button` (xs, border-radius 100px) ou `alert` |
| Timeline | Inner container: text + heading + button (badge) + tech tag row |
| Painel de contato | Container: text-editor rows + button links + alert + CTA |
| Pricing card | Inner container: text + heading + divider + `icon-list` + button |
| Testimonial | Widget `testimonial` nativo ou image + text-editor + heading |
| Logo cloud | `image-carousel` com 5+ slides |

## Formatos de Importação

### Método 1: .json Individual (Sempre Funciona)
- Sem plugins necessários
- Elementor → Templates → Saved Templates → Import
```json
{
  "version": "0.4",
  "title": "Page Name",
  "type": "page",
  "content": [...],
  "page_settings": {},
  "settings": {}
}
```

### Método 2: ZIP Nativo Elementor
- Elementor → Tools → Import Kit
- manifest.json + pastas (uma por template, cada com template.json)

### Método 3: ZIP Envato Template Kit
- Requer plugin "Template Kit Import"
- kit.json + pasta templates/
- ⚠️ Usar apenas se cliente tem o plugin instalado

**Recomendação padrão:** Método 1 (sempre funciona)

## Mapeamento CSS → Elementor

| CSS | Elementor |
|---|---|
| padding/margin | `"padding"`, `"_margin"` com top/right/bottom/left |
| font-family/size/weight | `"typography_font_family"`, `"typography_font_size"`, `"typography_font_weight"` |
| background-color | `"background_color"` (hex) |
| border | `"border_width"`, `"border_color"`, `"border_radius"` |
| box-shadow | `"box_shadow_box_shadow"` com horizontal, vertical, blur, spread, color |
| Responsivo | sufixo `_tablet`, `_mobile` |

## Estilos Globais do Kit

Sempre gerar com valores da marca; importar PRIMEIRO antes dos templates de página:
- Cores do sistema: primary, secondary, text, accent
- Cores customizadas: background, card, muted, border
- Tipografia: sizing, weight, line-height, letter-spacing

## Checklist de Output

- [ ] Zero HTML widgets para conteúdo visual (JS apenas)
- [ ] Navegação, badges, timelines, pricing usam widgets nativos
- [ ] Sem comentários JSON ou vírgulas finais
- [ ] Cada elemento: ID hex único 8 chars + `"elements": []`
- [ ] Top-level: `"isInner": false`; aninhado: `"isInner": true`
- [ ] Cores apenas em hex
- [ ] Breakpoints responsivos: `_tablet`, `_mobile`
- [ ] Um .json por página + global-styles.json
- [ ] Features Pro sinalizadas (sticky, Theme Builder, CSS customizado)
