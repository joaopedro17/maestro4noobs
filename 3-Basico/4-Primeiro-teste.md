# Primeiro Teste

Vamos criar um teste completo do zero! Usaremos o aplicativo de demonstração do Maestro para este exemplo.

## Preparação

Certifique-se que você tem:
- Maestro CLI instalado (`maestro --version`)
- Um emulador/dispositivo conectado (`adb devices` ou simulador iOS aberto)
- Um aplicativo instalado para testar

## Criando a Estrutura do Projeto

```bash
mkdir meu-primeiro-projeto-maestro
cd meu-primeiro-projeto-maestro
mkdir flows subflows
```

## Escrevendo o Primeiro Flow

Crie o arquivo `flows/login.yaml`:

```yaml
appId: com.exemplo.app
---
# Passo 1: Abre o app do zero
- launchApp:
    clearState: true

# Passo 2: Verifica que estamos na tela de login
- assertVisible: "E-mail"
- assertVisible: "Senha"

# Passo 3: Preenche o formulário
- tapOn: "E-mail"
- inputText: "usuario@email.com"

- tapOn: "Senha"
- inputText: "Senha@123"

# Passo 4: Faz o login
- tapOn: "Entrar"

# Passo 5: Verifica que o login funcionou
- assertVisible: "Início"
- assertNotVisible: "E-mail"
```

## Executando o Teste

```bash
maestro test flows/login.yaml
```

Você verá uma saída parecida com:

```
Running "flows/login.yaml"
  ✅ Launch app "com.exemplo.app"
  ✅ Assert visible "E-mail"
  ✅ Assert visible "Senha"
  ✅ Tap on "E-mail"
  ✅ Input text
  ✅ Tap on "Senha"
  ✅ Input text
  ✅ Tap on "Entrar"
  ✅ Assert visible "Início"
  ✅ Assert not visible "E-mail"

Flow Successful
```

## Executando Todos os Flows

```bash
maestro test flows/
```

## Adicionando Mais Cenários

Crie `flows/login-invalido.yaml`:

```yaml
appId: com.exemplo.app
---
- launchApp:
    clearState: true

- tapOn: "E-mail"
- inputText: "usuario@email.com"

- tapOn: "Senha"
- inputText: "senha-errada"

- tapOn: "Entrar"

# Verifica mensagem de erro
- assertVisible: "E-mail ou senha inválidos"
- assertNotVisible: "Início"
```

## Boas Práticas para Testes

- **Um cenário por flow**: cada arquivo testa um comportamento específico
- **Nomes descritivos**: `login-sucesso.yaml`, `login-senha-invalida.yaml`
- **Use `clearState: true`**: garante que os testes não dependam do estado anterior
- **Assertions no início e no fim**: verifique o estado inicial e o resultado final
- **Evite hardcoded waits**: prefira `assertVisible` com timeout customizado

---

> Ir para: [Subflows](../4-Intermediario/1-Subflows.md)
