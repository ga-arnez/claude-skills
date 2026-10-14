---
name: semantic-commits
description: Gera mensagens de commit no padrão Conventional Commits. Use quando o usuário pedir mensagem de commit, resumir mudanças para commit, ou mencionar "commit"/"git commit".
---

# Skill: Gerador de Commits Semânticos

## Quando usar
Sempre que o usuário pedir para gerar uma mensagem de commit, resumir mudanças para um commit, ou mencionar "commit", "git commit" ou "mensagem de commit".

## Instruções

Ao gerar mensagens de commit, siga sempre o padrão **Conventional Commits**:

```
<tipo>(<escopo opcional>): <descrição curta>

<corpo opcional>

<rodapé opcional>
```

### Tipos permitidos
- `feat` — nova funcionalidade
- `fix` — correção de bug
- `docs` — apenas documentação
- `style` — formatação, sem mudança de lógica
- `refactor` — refatoração sem nova feature ou fix
- `test` — adição ou correção de testes
- `chore` — tarefas de build, configs, dependências

### Regras
1. Descrição curta em **letras minúsculas**, máximo 72 caracteres
2. Use o **imperativo**: "adiciona", "corrige", "remove" (não "adicionado" ou "adicionando")
3. Não termine com ponto final
4. Se houver breaking change, adicione `BREAKING CHANGE:` no rodapé

### Exemplos

```
feat(auth): adiciona login com Google OAuth

fix(api): corrige timeout na chamada de pagamentos

docs: atualiza README com instruções de instalação

refactor(cart): simplifica lógica de cálculo de frete

feat(user)!: remove suporte a autenticação legada

BREAKING CHANGE: o endpoint /login/v1 foi removido, use /login/v2
```

## Comportamento esperado

Quando o usuário disser algo como:
- *"Gera um commit para isso"*
- *"Qual seria a mensagem de commit?"*
- *"Me ajuda com o commit dessas mudanças"*

Você deve:
1. Analisar as mudanças descritas ou os arquivos mencionados
2. Identificar o tipo correto
3. Sugerir 2 ou 3 opções de mensagem de commit
4. Explicar brevemente o motivo da escolha