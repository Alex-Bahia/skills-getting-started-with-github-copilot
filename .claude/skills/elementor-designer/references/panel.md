# Elementor V4 Panel Reference

## Estrutura das Abas

Todo widget V4 tem três abas consistentes:

1. **General** — HTML semântico, links, IDs, atributos customizados, display conditions
2. **Style** — Propriedades visuais: layout, spacing, tipografia, efeitos
3. **Interactions** — Animações por page load ou scroll

## Aba Style — Propriedades

| Grupo | Campos |
|---|---|
| **Layout** | Display mode, direction, alignment, gap, wrapping (Flexbox) |
| **Spacing** | Margin e padding com unidades: px, %, vw, vh, em, rem |
| **Size** | Width, height, overflow, aspect ratio |
| **Typography** | Font, weight, size, color, opções avançadas de texto |
| **Background & Border** | Color pickers com suporte a variáveis |
| **Effects** | Opacity, shadows, transforms, transitions, filters, backdrop |

⚠️ **Atenção:** alguns widgets V4 têm padding padrão de 10px — sempre verificar e zerar se desnecessário.

## Widgets Atômicos V4 (Recomendados)

| Widget | Uso |
|---|---|
| **Flexbox** | Seção/container principal |
| **Heading** | Títulos semânticos |
| **Paragraph** | Blocos de texto |
| **Image** | Imagens com controle de aspect ratio |
| **Button** | CTAs e links |
| **SVG** | Ícones vetoriais |
| **Divider** | Separadores (preferir Div block para mais controle) |
| **YouTube** | Vídeos embedados |
| **Tabs** | Conteúdo em abas |

## Class Manager

- Cria e gerencia classes reutilizáveis com variantes de estado: Normal, Hover, Focus, Active
- Aplicar classes antes de overrides locais
- Estilos locais marcados como `← LOCAL`

## Variables Manager

- Define CSS custom properties (`--nome-variavel`)
- Usar para cores, tipografia e spacing escaláveis
- Obrigatório para projetos com dark mode
