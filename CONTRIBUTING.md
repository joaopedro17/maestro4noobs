# Como Contribuir

Obrigado por querer contribuir com o Maestro4Noobs! Este documento explica como participar do projeto.

## Tipos de Contribuição

- Correção de erros de escrita ou código
- Melhoria em explicações existentes
- Adição de novos exemplos práticos
- Criação de novos módulos ou aulas
- Tradução ou revisão de conteúdo

## Fluxo de Trabalho

1. Faça um **fork** do repositório
2. Crie uma **branch** com o prefixo adequado:
   - `feat/nome-da-funcionalidade` — para novo conteúdo
   - `fix/descricao-do-erro` — para correções
   - `docs/descricao-da-melhoria` — para melhorias na documentação
   - `update/nome-do-conteudo` — para atualizações de conteúdo existente
3. Faça suas alterações
4. Crie um **commit** com mensagem descritiva (veja padrão abaixo)
5. Abra um **Pull Request** para a branch `main`

## Padrão de Commit

```
tipo: descrição curta em português

Exemplos:
feat: adiciona aula sobre subflows
fix: corrige exemplo de assertVisible
docs: melhora explicação sobre variáveis de ambiente
update: atualiza sintaxe para Maestro 1.x
```

## Padrões de Conteúdo

- Todo conteúdo deve ser escrito em **Português (Brasil)**
- Exemplos de código devem usar **YAML** (sintaxe do Maestro)
- Use a versão **Maestro 1.x** (mais recente) nos exemplos
- Mantenha a numeração sequencial dos arquivos (ex: `1-`, `2-`, `3-`)
- Cada arquivo deve ter um rodapé de navegação apontando para o próximo conteúdo
- Inclua sempre um bloco de **boas práticas** quando relevante

## Estrutura de Arquivo

Cada arquivo `.md` deve seguir este padrão:

```markdown
# Título da Aula

Introdução breve sobre o conteúdo.

## Seção Principal

Conteúdo detalhado com exemplos.

## Boas Práticas

- Dica 1
- Dica 2

---

> Ir para: [Próxima Aula](link-para-proxima.md)
```

## Dúvidas

Abra uma [issue](../../issues) no repositório antes de começar grandes alterações para alinharmos a abordagem.
