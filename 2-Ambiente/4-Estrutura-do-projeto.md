# Estrutura do Projeto

Não existe uma estrutura obrigatória para projetos Maestro, mas as boas práticas recomendam uma organização clara. Veja a estrutura sugerida:

```
meu-projeto-maestro/
├── flows/
│   ├── login.yaml
│   ├── cadastro.yaml
│   └── checkout.yaml
├── subflows/
│   ├── _login-helper.yaml
│   └── _navegacao.yaml
├── .env
├── .env.staging
└── README.md
```

## Diretórios

### `flows/`

Contém os flows principais do seu projeto — cada arquivo representa um cenário de teste.

```yaml
# flows/login.yaml
appId: com.meuapp.android
---
- runFlow: ../subflows/_login-helper.yaml
- assertVisible: "Dashboard"
```

### `subflows/`

Contém flows auxiliares reutilizáveis. A convenção de usar `_` no início do nome indica que são helpers e não devem ser executados diretamente.

```yaml
# subflows/_login-helper.yaml
- tapOn: "E-mail"
- inputText: ${EMAIL}
- tapOn: "Senha"
- inputText: ${SENHA}
- tapOn: "Entrar"
```

### `.env`

Arquivo de variáveis de ambiente para configuração local:

```env
EMAIL=usuario@email.com
SENHA=minha-senha
BASE_URL=https://api.meuapp.com
```

> **Nunca commite o `.env` com dados sensíveis.** Adicione ao `.gitignore`.

## Executando os Testes

### Executar um flow específico

```bash
maestro test flows/login.yaml
```

### Executar todos os flows de um diretório

```bash
maestro test flows/
```

### Executar com variáveis de ambiente

```bash
maestro test flows/login.yaml --env EMAIL=teste@email.com --env SENHA=123456
```

### Executar com arquivo .env

```bash
maestro test flows/login.yaml --env-file .env
```

## Inspecionando Elementos

Para descobrir os identificadores dos elementos na tela:

```bash
maestro hierarchy
```

Isso exibe a árvore de elementos da tela atual do dispositivo/emulador conectado.

---

> Ir para: [Dicas Gerais](5-Dicas-gerais.md)
