# CI/CD com Maestro

Integrar os testes Maestro ao pipeline de CI/CD garante que regressões sejam detectadas automaticamente a cada mudança no código.

## GitHub Actions com Android

```yaml
# .github/workflows/testes-android.yml
name: Testes Android com Maestro

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  testes-android:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout do código
        uses: actions/checkout@v4

      - name: Configurar Java
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Instalar Maestro
        run: |
          curl -Ls "https://get.maestro.mobile.dev" | bash
          echo "$HOME/.maestro/bin" >> $GITHUB_PATH

      - name: Iniciar emulador Android
        uses: reactivecircus/android-emulator-runner@v2
        with:
          api-level: 34
          arch: x86_64
          profile: Pixel 6
          script: |
            # Instala o APK
            adb install app/build/outputs/apk/debug/app-debug.apk

            # Executa os testes
            maestro test flows/ --format junit --output resultado.xml

      - name: Publicar resultados
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Resultados Maestro
          path: resultado.xml
          reporter: java-junit
```

## GitHub Actions com iOS (macOS)

```yaml
name: Testes iOS com Maestro

on:
  push:
    branches: [main]

jobs:
  testes-ios:
    runs-on: macos-latest

    steps:
      - uses: actions/checkout@v4

      - name: Instalar Maestro
        run: |
          curl -Ls "https://get.maestro.mobile.dev" | bash
          echo "$HOME/.maestro/bin" >> $GITHUB_PATH

      - name: Iniciar simulador iOS
        run: |
          xcrun simctl boot "iPhone 15"
          sleep 30

      - name: Instalar app no simulador
        run: |
          xcrun simctl install booted MeuApp.app

      - name: Executar testes
        run: maestro test flows/ --format junit --output resultado.xml

      - name: Publicar resultados
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Resultados iOS
          path: resultado.xml
          reporter: java-junit
```

## Usando Secrets no CI

```yaml
- name: Executar testes com credenciais
  env:
    EMAIL: ${{ secrets.TEST_EMAIL }}
    SENHA: ${{ secrets.TEST_SENHA }}
  run: |
    maestro test flows/ \
      --env EMAIL=$EMAIL \
      --env SENHA=$SENHA \
      --format junit \
      --output resultado.xml
```

## Boas Práticas para CI

- Sempre use `--format junit` para resultados compatíveis com CI
- Configure retentativas para flakiness: `maestro test flows/ --max-retries 2`
- Armazene o APK/IPA como artifact para reuso
- Use cache de dependências para acelerar builds

---

> Ir para: [Maestro Cloud](4-Cloud-testing.md)
