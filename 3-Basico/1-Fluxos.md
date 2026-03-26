# Fluxos (Flows)

Um **flow** no Maestro é um arquivo YAML que descreve uma sequência de ações que um usuário realizaria em um aplicativo mobile. É o conceito central do Maestro.

## Estrutura Básica de um Flow

```yaml
# Identificador único do aplicativo (obrigatório)
appId: com.meuapp.android

# Separador obrigatório entre o cabeçalho e os passos
---

# Lista de passos (comandos)
- launchApp
- tapOn: "Entrar"
- assertVisible: "Dashboard"
```

### Partes do Flow

| Seção | Descrição |
|-------|-----------|
| `appId` | Bundle ID do app (iOS) ou package name (Android) — obrigatório |
| `---` | Separador YAML entre o cabeçalho e os passos |
| Lista de passos | Cada item da lista é uma ação a ser executada |

## Metadados Opcionais

Você pode adicionar informações extras no cabeçalho:

```yaml
appId: com.meuapp.android
name: "Fluxo de Login"
tags:
  - login
  - autenticacao
---
- launchApp
- tapOn: "E-mail"
```

## Exemplos de Flows

### Flow de Login

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn:
    text: "E-mail"
- inputText: "usuario@email.com"
- tapOn:
    text: "Senha"
- inputText: "minha-senha"
- tapOn:
    text: "Entrar"
- assertVisible: "Olá, Usuário!"
```

### Flow de Cadastro

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn: "Criar conta"
- inputText: "Novo Usuário"
- tapOn: "Próximo"
- inputText: "novo@email.com"
- tapOn: "Próximo"
- inputText: "Senha@123"
- tapOn: "Cadastrar"
- assertVisible: "Cadastro realizado com sucesso!"
```

## Executando um Flow

```bash
# Executar um flow específico
maestro test flows/login.yaml

# Executar com output detalhado
maestro test flows/login.yaml --debug-output ./debug
```

## Descobrindo o appId

**Android:**
```bash
adb shell pm list packages | grep nomedoapp
```

**iOS:**
```bash
xcrun simctl listapps booted | grep -i nomedoapp
```

---

> Ir para: [Comandos Básicos](2-Comandos-basicos.md)
