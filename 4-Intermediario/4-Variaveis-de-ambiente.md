# Variáveis de Ambiente

Variáveis de ambiente permitem parametrizar seus flows, separando dados de configuração do código dos testes.

## Sintaxe Básica

Use `${NOME_DA_VARIAVEL}` para referenciar variáveis nos flows:

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn: "E-mail"
- inputText: ${EMAIL}
- tapOn: "Senha"
- inputText: ${SENHA}
- tapOn: "Entrar"
```

## Passando via CLI

```bash
# Uma variável
maestro test flows/login.yaml --env EMAIL=usuario@email.com

# Múltiplas variáveis
maestro test flows/login.yaml \
  --env EMAIL=usuario@email.com \
  --env SENHA=Senha@123 \
  --env AMBIENTE=staging
```

## Arquivo .env

Crie um arquivo `.env` na raiz do projeto:

```env
# .env
EMAIL=usuario@email.com
SENHA=Senha@123
AMBIENTE=staging
BASE_URL=https://staging.meuapp.com
```

Execute com o arquivo:

```bash
maestro test flows/ --env-file .env
```

## Ambientes Diferentes

Crie arquivos `.env` por ambiente:

```
.env.dev
.env.staging
.env.prod
```

```bash
# Testes em staging
maestro test flows/ --env-file .env.staging

# Testes em produção
maestro test flows/ --env-file .env.prod
```

## Valores Padrão

Defina valores padrão diretamente no flow para variáveis opcionais:

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn: "E-mail"
# Se EMAIL não for definido, usa o valor padrão
- inputText: ${EMAIL:-usuario.padrao@email.com}
```

## Exemplo: Multi-ambiente

```yaml
# flows/smoke-test.yaml
appId: com.meuapp.android
---
- launchApp:
    clearState: true

- tapOn: "E-mail"
- inputText: ${EMAIL}
- tapOn: "Senha"
- inputText: ${SENHA}
- tapOn: "Entrar"

- assertVisible: ${MENSAGEM_BOAS_VINDAS:-Bem-vindo!}
```

```env
# .env.staging
EMAIL=qa@empresa.com
SENHA=QA@Senha123
MENSAGEM_BOAS_VINDAS=Bem-vindo ao Staging!
```

## Segurança

- **Nunca commite** arquivos `.env` com credenciais reais
- Adicione ao `.gitignore`: `.env`, `.env.*`, `!.env.example`
- Crie um `.env.example` com valores fictícios como documentação
- Em CI/CD, use **secrets** da plataforma (GitHub Secrets, GitLab Variables)

---

> Ir para: [Organização de Flows](5-Organizacao.md)
