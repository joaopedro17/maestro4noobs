# Configuração no Windows

> **Atenção**: O Maestro não suporta iOS no Windows (iOS requer macOS com Xcode). Para testes Android, você pode usar WSL2 ou um dispositivo/emulador conectado via ADB.

## Opção 1: Instalação via WSL2 (Recomendada)

O WSL2 (Windows Subsystem for Linux) permite rodar o Maestro em ambiente Linux dentro do Windows.

### 1. Instale o WSL2

Abra o PowerShell como Administrador e execute:

```powershell
wsl --install
```

Reinicie o computador e conclua a instalação do Ubuntu.

### 2. Instale o Maestro no WSL2

Dentro do terminal WSL2:

```bash
curl -Ls "https://get.maestro.mobile.dev" | bash
export PATH="$PATH:$HOME/.maestro/bin"
echo 'export PATH="$PATH:$HOME/.maestro/bin"' >> ~/.bashrc
source ~/.bashrc
```

### 3. Configure o ADB no WSL2

Instale as ferramentas de plataforma Android:

```bash
sudo apt-get update
sudo apt-get install -y android-tools-adb
```

Para conectar ao emulador rodando no Windows, use o IP do host:

```bash
adb connect 127.0.0.1:5555
adb devices
```

## Opção 2: Dispositivo Android físico

1. Ative o **Modo Desenvolvedor** no dispositivo Android
2. Ative a opção **Depuração USB** nas opções do desenvolvedor
3. Conecte o dispositivo via USB
4. Verifique a conexão:

```bash
adb devices
```

## Instalando o Android Studio no Windows

Para usar o emulador Android no Windows:

1. Baixe o [Android Studio](https://developer.android.com/studio)
2. Durante a instalação, inclua o **Android Virtual Device (AVD)**
3. Após instalar, crie um AVD pelo **Device Manager**
4. Inicie o emulador

## Verificando a instalação

```bash
maestro --version
maestro hierarchy
```

---

> Ir para: [Configuração no Linux](3-Ambiente-linux.md)
