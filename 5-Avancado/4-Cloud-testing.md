# Maestro Cloud

O **Maestro Cloud** é um serviço de execução de testes em infraestrutura na nuvem, eliminando a necessidade de gerenciar emuladores localmente no CI.

## Por que usar o Maestro Cloud?

- Sem necessidade de configurar emuladores no CI
- Execução em dispositivos reais e virtuais
- Relatórios detalhados com screenshots e vídeos
- Integração direta com GitHub, GitLab e outros
- Escalabilidade automática

## Configurando o Maestro Cloud

### 1. Crie uma conta

Acesse [cloud.mobile.dev](https://cloud.mobile.dev/) e crie sua conta.

### 2. Instale o CLI

```bash
# O mesmo CLI do Maestro inclui o comando cloud
maestro --version
```

### 3. Faça o upload do app e execute

```bash
# Upload do APK e execução dos flows
maestro cloud \
  --apiKey $MAESTRO_CLOUD_API_KEY \
  app.apk \
  flows/
```

Para iOS:

```bash
maestro cloud \
  --apiKey $MAESTRO_CLOUD_API_KEY \
  MeuApp.ipa \
  flows/
```

## Integração com GitHub Actions

```yaml
# .github/workflows/maestro-cloud.yml
name: Testes no Maestro Cloud

on:
  pull_request:
    branches: [main]

jobs:
  testes-cloud:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Instalar Maestro
        run: |
          curl -Ls "https://get.maestro.mobile.dev" | bash
          echo "$HOME/.maestro/bin" >> $GITHUB_PATH

      - name: Baixar último APK
        run: |
          # Substitua pelo seu comando de download do APK
          wget -O app.apk "${{ secrets.APK_DOWNLOAD_URL }}"

      - name: Executar testes no Maestro Cloud
        run: |
          maestro cloud \
            --apiKey ${{ secrets.MAESTRO_CLOUD_API_KEY }} \
            app.apk \
            flows/
```

## Relatórios e Resultados

Após a execução, o Maestro Cloud gera:

- **Relatório HTML** com todos os testes
- **Screenshots** de cada passo
- **Vídeo** completo da execução
- **Logs detalhados** de falhas
- **Status de CI** integrado ao PR

## Planos do Maestro Cloud

| Plano | Execuções | Dispositivos | Preço |
|-------|-----------|-------------|-------|
| Free | 100/mês | Virtual | Gratuito |
| Pro | Ilimitado | Virtual + Real | Pago |
| Enterprise | Ilimitado | Personalizado | Contato |

## Boas Práticas

- Use o Cloud para PRs e builds de CI
- Execute localmente durante o desenvolvimento
- Armazene o `MAESTRO_CLOUD_API_KEY` como secret no GitHub/GitLab
- Configure alertas para falhas nos testes via notificações do Cloud

---

> Ir para: [EAS Workflows](5-EAS-Workflows.md)
