# StarIA Gestor — plugins da Startwo para o Claude

Catálogo de plugins da Startwo. O plugin **StarIA Gestor** traz 75 comandos `/staria-*` para administrar a loja com o ERP StarIA: resumo do dia, vendas, mercadorias, estoque, compras, financeiro, encartes, seções, alertas e relatórios em PDF.

Pré-requisito: conector **StarIA** conectado na conta do Claude.

## Instalar no Claude (app, navegador ou desktop)

1. Abra **Customize** na barra lateral e vá até **Plugins**.
2. Clique em **Add** › **Add marketplace** › **Add from a repository**.
3. Informe `JaderDestefani/StarIA_Gestor`.
4. Na aba **Discover**, escolha **StarIA Gestor** e clique em **Add**.

## Instalar no Claude Code

```
/plugin marketplace add JaderDestefani/StarIA_Gestor
/plugin install staria-gestor@startwo
```

## Atualizar

Cada versão nova é publicada neste repositório. No Claude Code: `/plugin marketplace update startwo`. A lista de comandos e as novidades de cada versão estão em `plugins/staria-gestor/README.md`.

Depois de instalar, digite `/staria-ajuda` para ver todos os comandos.
