# Configuração no macOS

## Instalando o Maestro CLI

O Maestro é instalado via linha de comando. Abra o **Terminal** e execute:

```bash
curl -Ls "https://get.maestro.mobile.dev" | bash
```

Após a instalação, adicione o Maestro ao PATH (caso não seja feito automaticamente):

```bash
export PATH="$PATH:$HOME/.maestro/bin"
```

Para tornar isso permanente, adicione a linha acima ao seu `~/.zshrc` ou `~/.bash_profile`:

```bash
echo 'export PATH="$PATH:$HOME/.maestro/bin"' >> ~/.zshrc
source ~/.zshrc
```

### Verificando a instalação

```bash
maestro --version
```

Você deve ver algo como `Maestro v1.x.x`.

## Configurando para Android

### 1. Instale o Android Studio

Baixe e instale o [Android Studio](https://developer.android.com/studio). Ele inclui o Android SDK e o emulador.

### 2. Configure as variáveis de ambiente

```bash
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

Adicione ao `~/.zshrc` e execute `source ~/.zshrc`.

### 3. Verifique o ADB

```bash
adb devices
```

Inicie um emulador Android pelo Android Studio e execute o comando novamente. Você deverá ver o dispositivo listado.

## Configurando para iOS (apenas macOS)

### 1. Instale o Xcode

Instale o **Xcode** pela App Store ou pelo [site oficial da Apple](https://developer.apple.com/xcode/).

### 2. Instale os Command Line Tools

```bash
xcode-select --install
```

### 3. Abra o Simulador iOS

```bash
open -a Simulator
```

### 4. Verifique a conexão

```bash
maestro hierarchy
```

Se o simulador estiver rodando com um app aberto, você verá a hierarquia de elementos da tela.

## Executando seu primeiro flow

Com um emulador/simulador em execução e um app instalado, crie o arquivo `login.yaml`:

```yaml
appId: com.meuapp.ios  # ou com.meuapp.android
---
- launchApp
- assertVisible: "Tela inicial"
```

Execute:

```bash
maestro test login.yaml
```

---

> Ir para: [Configuração no Windows](2-Ambiente-windows.md)
