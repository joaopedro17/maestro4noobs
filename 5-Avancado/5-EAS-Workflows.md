# Testes E2E com EAS Workflows (Expo)

O **EAS Workflows** é uma solução de CI/CD nativa da Expo que integra build e testes Maestro em um único pipeline — sem precisar configurar emuladores manualmente no GitHub Actions.

> **Funciona em projetos Expo padrão**, não é exclusivo de monorepos.

## Quando usar essa alternativa?

| Situação | Recomendação |
|----------|-------------|
| Projeto Expo/React Native com EAS configurado | EAS Workflows |
| Projeto React Native sem Expo | GitHub Actions (ver [CI/CD](3-CI-CD.md)) |
| Quer evitar configurar emuladores no CI | EAS Workflows |
| Precisa de controle total do pipeline | GitHub Actions |

## Pré-requisitos

- Conta na [Expo](https://expo.dev/)
- Projeto configurado com `eas.json`
- Repositório GitHub vinculado ao projeto EAS
- EAS CLI instalado: `npm install -g eas-cli`

## Configuração

### 1. Estrutura dos flows Maestro

Crie um diretório `.maestro/` na raiz do projeto (mesmo nível do `eas.json`):

```
meu-projeto/
├── .maestro/
│   ├── home.yml
│   └── expand_test.yml
├── eas.json
└── app.json
```

**Exemplo: `.maestro/home.yml`**

```yaml
appId: com.seuapp.id
---
- launchApp
- assertVisible: "Bem-vindo!"
```

**Exemplo: `.maestro/expand_test.yml`**

```yaml
appId: com.seuapp.id
---
- launchApp
- tapOn: "Explorar.*"
- assertVisible: "Navegação baseada em arquivos.*"
```

### 2. Perfil de build no `eas.json`

Adicione um perfil específico para testes e2e:

```json
{
  "build": {
    "e2e-test": {
      "withoutCredentials": true,
      "ios": {
        "simulator": true
      },
      "android": {
        "buildType": "apk"
      }
    }
  }
}
```

O perfil `withoutCredentials: true` evita a necessidade de certificados de assinatura, acelerando o build para testes.

### 3. Workflow para Android

Crie `.eas/workflows/e2e-test-android.yml`:

```yaml
name: e2e-test-android

on:
  pull_request:
    branches: ['*']

jobs:
  build_android_for_e2e:
    type: build
    params:
      platform: android
      profile: e2e-test

  maestro_test:
    needs: [build_android_for_e2e]
    type: maestro
    params:
      build_id: ${{ needs.build_android_for_e2e.outputs.build_id }}
      flow_path:
        - .maestro/home.yml
        - .maestro/expand_test.yml
```

### 4. Workflow para iOS

Crie `.eas/workflows/e2e-test-ios.yml`:

```yaml
name: e2e-test-ios

on:
  pull_request:
    branches: ['*']

jobs:
  build_ios_for_e2e:
    type: build
    params:
      platform: ios
      profile: e2e-test

  maestro_test:
    needs: [build_ios_for_e2e]
    type: maestro
    params:
      build_id: ${{ needs.build_ios_for_e2e.outputs.build_id }}
      flow_path:
        - .maestro/home.yml
        - .maestro/expand_test.yml
```

## Executando os testes

### Manualmente via CLI

```bash
# Android
npx eas-cli@latest workflow:run .eas/workflows/e2e-test-android.yml

# iOS
npx eas-cli@latest workflow:run .eas/workflows/e2e-test-ios.yml
```

### Automaticamente em Pull Requests

Com o campo `on.pull_request` configurado, o workflow dispara automaticamente quando um PR é aberto ou atualizado.

## Como funciona por baixo dos panos

```
PR aberto
    ↓
EAS Workflow inicia
    ↓
Job "build": gera APK/Simulator build via EAS Build
    ↓
Job "maestro_test": recebe o build_id e executa os flows Maestro
    ↓
Resultado aparece no PR e no painel da Expo
```

O EAS cuida de provisionar o emulador, instalar o app e rodar o Maestro — tudo gerenciado pela infraestrutura da Expo.

## Boas Práticas

- Mantenha os flows do `.maestro/` simples e focados em fluxos críticos
- Use `assertVisible` com regex (`.*`) para textos que podem variar
- O perfil `e2e-test` é separado do perfil de produção: não precisa de credenciais de assinatura
- Combine com o `on.pull_request` para garantir que nenhum PR quebre o app

---

> Parabéns! Você concluiu o Maestro4Noobs! Volte ao [README](../README.md) para revisar o conteúdo ou contribua com melhorias.
