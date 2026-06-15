# Elementor V4 — Troubleshooting

## Layout

| Problema | Solução |
|---|---|
| Gap não funciona | Gap só funciona em pais Flexbox. Se o pai é Div block (não-flex), gap não tem efeito |
| Elemento empilhando errado | Verificar `flex-direction` do pai e `flex-wrap` |
| Overflow inesperado | Verificar `overflow: hidden` no container pai |
| Sticky não funciona | Elemento precisa ter `position: sticky` e pai sem `overflow: hidden` |
| Colunas desiguais | Verificar `flex-grow`, `flex-shrink` e `flex-basis` de cada coluna |

## Spacing

| Problema | Solução |
|---|---|
| Padding assimétrico | Verificar se o link de valores está ativado no painel |
| Margens irregulares entre elementos | Remover margens dos filhos; usar `gap` no pai Flexbox |

## Tipografia

| Problema | Solução |
|---|---|
| Tamanho de fonte não atualiza | Conflito de classes — verificar se há classe global sobrescrevendo |
| Cor aparece como hex em vez de variável | Reselecionar a variável no color picker |
| Line-height desajustado | Ajustar em Style → Typography → Line Height (unidade: em) |

## Dark Mode

- Verificar se Design Tokens têm overrides `[data-theme="dark"]`
- Botão toggle deve ter atributo `data-theme-toggle`
- Confirmar que TODAS as cores usam variáveis — nenhuma hardcoded

## Animações

| Problema | Solução |
|---|---|
| Animação não dispara | Verificar trigger: scroll-based vs. page load — são configurados separadamente |
| Animação na direção errada | Conferir offset e direction em Interactions |
| Múltiplas animações conflitando | Gerenciar ordem e delay em Interactions — apenas uma `entrance` por elemento |

## Custom Code

| Problema | Solução |
|---|---|
| Snippet CSS não aplica | Verificar se está ativado em Site Settings → Custom Code |
| Variável CSS não resolve | Confirmar prefixo `--` duplo e que a variável está definida em `:root` ou no Variables Manager |
| Código não encontra elemento | Confirmar seletor CSS — usar classe BEM ou ID, não `.elementor-widget-container` |
