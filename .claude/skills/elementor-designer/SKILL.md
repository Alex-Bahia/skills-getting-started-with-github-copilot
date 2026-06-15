# Elementor Pro V4 — Visual Designer Skill

Skill focada em designers usando o editor visual do Elementor Pro V4 — não desenvolvedores PHP. Fornece instruções por aba/campo do painel, não em CSS ou código.

## Mudanças Principais do V3 para V4

- **Flexbox** substitui Container como widget primário de layout
- Painel agora usa abas **General / Style / Interactions** (antes: Content/Style/Advanced)
- **Class Manager** e **Variables Manager** substituem abordagens antigas
- Widgets atômicos V4: Flexbox, Heading, Paragraph, Image, Button, SVG, Divider, YouTube, Tabs
- Evitar widgets legados V3 como Container e Text Editor em projetos novos

## Hierarquia de Decisão

Antes de implementar qualquer estilo, seguir esta ordem:
1. Usar classes existentes
2. Usar Style settings do painel
3. Referenciar variáveis CSS
4. Criar classe global se necessário
5. Criar variável se necessário
6. Custom code apenas como último recurso

## Formato de Instrução

Sempre fornecer instruções organizadas por ordem de aba: **General → Style → Interactions**, usando nomes de campos do painel (não sintaxe CSS). Indicar se o setting vem de classe global, override local, ou nova classe a criar.

## Erros Comuns a Evitar

- Referenciar aba "Advanced" (não existe no V4)
- Hardcodar cores em projetos com dark mode toggle (usar variáveis)
- Confundir Flexbox com o widget Container antigo
- Usar alturas fixas em vez de `flex-grow` para layouts responsivos

## Estrutura de Layout Recomendada

- Flexbox como seção mais externa
- Máximo 4 níveis de aninhamento
- Distribuir espaçamento via `gap` no pai, não margens nos filhos
- **Breakpoints:** Desktop (base) → Tablet (1024px) → Phone (767px)

## Dark Mode

- Todas as cores devem usar variáveis CSS (nunca hardcoded) quando há toggle de tema
- Valores light em `:root`
- Overrides dark em `[data-theme="dark"]`
- Botão toggle com atributo `data-theme-toggle`

## Referências

- `references/panel.md` — Referência completa do painel V4 (abas, campos, widgets)
- `references/patterns.md` — Padrões de layout prontos para uso
- `references/troubleshooting.md` — Problemas comuns e soluções
