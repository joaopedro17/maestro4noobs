# Configuração no Linux

> O Maestro no Linux suporta apenas Android (iOS requer macOS com Xcode).

## Instalando o Maestro CLI

Abra o terminal e execute:

```bash
curl -Ls "https://get.maestro.mobile.dev" | bash
```

Adicione ao PATH:

```bash
export PATH="$PATH:$HOME/.maestro/bin"
echo 'export PATH="$PATH:$HOME/.maestro/bin"' >> ~/.bashrc
source ~/.bashrc
```

Verifique:

```bash
maestro --version
```

## Configurando o Android

### 1. Instale dependências

```bash
sudo apt-get update
sudo apt-get install -y openjdk-17-jdk android-tools-adb
```

### 2. Instale o Android Studio

Baixe o [Android Studio para Linux](https://developer.android.com/studio) e extraia:

```bash
tar -xzf android-studio-*.tar.gz -C /opt/
/opt/android-studio/bin/studio.sh
```

### 3. Configure as variáveis de ambiente

```bash
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

Adicione ao `~/.bashrc` e execute `source ~/.bashrc`.

### 4. Crie um emulador

No Android Studio, vá em **Device Manager > Create Device**, escolha um hardware profile (ex: Pixel 6) e uma imagem do sistema (ex: API 34).

### 5. Inicie o emulador pela linha de comando

```bash
# Liste os AVDs disponíveis
emulator -list-avds

# Inicie um AVD
emulator -avd Pixel_6_API_34
```

### 6. Verifique a conexão

```bash
adb devices
```

## Dispositivo físico

1. Ative a **Depuração USB** no dispositivo Android
2. Conecte via USB
3. Execute `adb devices` para confirmar

## Rodando um teste

```bash
maestro test meu-flow.yaml
```

---

> Ir para: [Estrutura do Projeto](4-Estrutura-do-projeto.md)
