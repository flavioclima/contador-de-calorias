# Contador de calorias (skill para o Claude)

Skill que transforma o Claude num diário alimentar: você manda o que comeu (texto, foto do prato ou do rótulo) e ele calcula calorias, proteína, carboidrato e gordura, guarda no Notion, Google Planilhas ou arquivo, e responde com o total do dia e a porcentagem de cada meta. Quando você pede "relatório mensal", ele gera uma página com gráficos de calorias e proteína por dia, onde você toca em um ponto para ver as refeições daquele dia.

> As calorias são estimativas. Esta skill não substitui nutricionista ou médico.

## Como instalar no Claude (app/site)

1. Baixe o arquivo `contador-de-calorias.zip` (em *Releases* ou na raiz do repositório).
2. No Claude, vá em **Customize > Skills**, clique em **+** e escolha **Upload a skill**.
3. Envie o zip e ative a skill.
4. Na primeira conversa, diga algo como "vou começar a contar calorias". A skill pergunta onde guardar os dados e quais são as suas metas.

Para guardar no Notion ou no Google Planilhas, ative o conector correspondente no Claude. Sem conector, a skill usa um arquivo CSV/.xlsx.

## Outros usos

- **Claude Code:** copie a pasta `contador-de-calorias/` para `~/.claude/skills/`.
- **Outras IAs:** cole o conteúdo de `contador-de-calorias/SKILL.md` como instruções. Perde a ativação automática.

## Estrutura

```
contador-de-calorias/
  SKILL.md
```
