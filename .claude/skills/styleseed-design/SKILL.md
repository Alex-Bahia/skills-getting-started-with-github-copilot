# StyleSeed — Design Engine (69 Regras de Design)

Motor de design para Claude Code: ensina julgamento visual, não apenas dados. 69 regras, 48 componentes, 7 brand skins, 14 slash commands.

Objetivo: "Parar de entregar UI que parece gerada por máquina."

## Regras Fundamentais de Cor

- **Uma cor de destaque** — tudo o mais em escala de cinza
- Preto refinado = `#2A2A2A`, nunca `#000000`
- 5 níveis de cinza para hierarquia de texto: #2A2A2A → #9B9B9B
- Cor primária apenas para estados ativos/selecionados
- Fundos de cards: levemente mais claros que o fundo da página (escuro: mais claros; claro: ligeiramente off-white)
- Sombras com 4–8% de opacidade — quase invisíveis

## Tipografia (5 Níveis)

| Nível | Tamanho | Uso |
|---|---|---|
| Display | 48px | Números grandes de KPI |
| Title | 24px | Títulos de seção |
| Body | 16px | Conteúdo padrão |
| Label | 12px | Labels (uppercase + letter-spacing obrigatório) |
| Micro | 10px | Metadados, datas |

**Números grandes:** ratio 2:1 entre valor e unidade (ex: `48px` número + `24px` unidade). Sempre `whitespace-nowrap`.

## Layout (4 Tipos de Seção)

| Tipo | Descrição |
|---|---|
| A | Full card com `mx-6` |
| B | Grid com `px-6` |
| C | Carousel horizontal |
| D | Hero section |

**Regra do Skyline:** Alternar alturas de seção — nunca repetir o mesmo tipo consecutivamente. Cards altos (charts 280px) ao lado de compactos (carousels).

**Estrutura por página:**
- Mínimo 4 seções, máximo 7
- Largura máxima: `max-w-[430px]` centralizado
- Espaçamento entre seções: `space-y-6` (24px)
- Padding inferior (acima da nav): `pb-24`

## Regras de Cards

- **Todo conteúdo fica dentro de cards** — sem exceções para métricas, listas ou texto
- Radius: `rounded-2xl` (16px) universalmente
- Padding interno: 16px (sm) / 20px (md) / 24px (lg)

## Botões (7 Variantes, 4 Tamanhos)

- Primary CTA: altura 52px, `rounded-xl`
- Animação de press: 150ms, `scale(0.97)`
- Status indicators: mesma cor para dot e texto (consistência por cor)

## Dark Mode

- Brilho do card > brilho do fundo
- Usar borders em vez de sombras no dark
- Nunca inverter — recriar com valores próprios para dark

## Animações

- Dashboard charts/donut: 300ms
- Botões/interações: 150ms
- Sem motion scroll-linked
- Loading states: copiam o shape final do conteúdo, com 300ms de delay

## Ícones

- `strokeWidth` inversamente proporcional ao tamanho: ícones maiores ficam mais finos

## Proibições Absolutas (30+ Regras)

- ❌ Preto puro (`#000`)
- ❌ Conteúdo fora de cards
- ❌ Backgrounds coloridos com cor-chave
- ❌ Mesmo tipo de seção consecutiva
- ❌ Sombras com opacidade > 8%
- ❌ Gradientes decorativos sem propósito
- ❌ Typography sem hierarquia clara
- ❌ Botões sem estado de hover/active/focus
- ❌ Valores hardcoded de cor ou spacing

## Tokens Principais

```css
/* Spacing */
--space-section: 24px;   /* space-y-6 */
--space-card-sm: 16px;
--space-card-md: 20px;
--space-card-lg: 24px;

/* Radius */
--radius-card: 16px;     /* rounded-2xl */
--radius-button: 12px;   /* rounded-xl */

/* Shadows */
--shadow-card: 0 2px 8px rgba(0,0,0,0.06);
--shadow-elevated: 0 4px 16px rgba(0,0,0,0.08);

/* Opacity */
--opacity-watermark: 0.06;
--opacity-disabled: 0.38;
--opacity-selected: 1.0;
```

## Brand Skins Disponíveis

toss · stripe · linear · notion · raycast · arc · vercel + 58 marcas adicionais

## Slash Commands (14)

**Setup & Core:** `/ss-setup`, `/ss-page`, `/ss-component`, `/ss-pattern`

**Motion & Review:** `/ss-motion`, `/ss-review`, `/ss-tokens`, `/ss-lint`, `/ss-update`

**UX & A11y:** `/ss-flow`, `/ss-audit`, `/ss-copy`, `/ss-feedback`, `/ss-a11y`

## Integração com Elementor

As regras de design do StyleSeed se aplicam tanto a React quanto ao Elementor:

1. **Tokens CSS** → importar no Variables Manager do Elementor
2. **Regras de card** → aplicar via Class Manager (radius, padding, shadow)
3. **Tipografia de 5 níveis** → configurar em Site Settings → Typography do Elementor
4. **Skins de brand** → configurar uma paleta global antes de qualquer page template
5. **Regra do Skyline** → alternar seções Elementor por tipo e altura para ritmo visual

## Checklist Pré-Entrega

- [ ] Uma cor de destaque — resto em escala de cinza
- [ ] Hierarquia tipográfica com 5 níveis
- [ ] Todo conteúdo dentro de cards
- [ ] Nenhum tipo de seção repetido consecutivamente
- [ ] Sombras ≤ 8% opacidade
- [ ] Dark mode testado separadamente
- [ ] Todos os estados de botão implementados
- [ ] Sem valores hardcoded
