# Assertions

Assertions são verificações que confirmam que o app está no estado esperado. Se uma assertion falhar, o flow para e o teste é marcado como falho.

## assertVisible

Verifica que um elemento está visível na tela:

```yaml
# Por texto
- assertVisible: "Bem-vindo!"

# Por ID
- assertVisible:
    id: "mensagem_sucesso"

# Por texto com timeout customizado (ms)
- assertVisible:
    text: "Carregando dados..."
    timeout: 10000
```

## assertNotVisible

Verifica que um elemento **não** está visível:

```yaml
- assertNotVisible: "Mensagem de erro"

- assertNotVisible:
    id: "spinner_loading"
```

## assertTrue

Executa uma expressão JavaScript e verifica que o resultado é verdadeiro:

```yaml
# Verifica que o output contém um texto
- assertTrue:
    condition: ${output.texto.contains("Sucesso")}

# Verifica uma variável de ambiente
- assertTrue:
    condition: ${USUARIO != ""}
```

## assertWithAI (experimental)

Usa IA para verificar o estado da tela com linguagem natural:

```yaml
- assertWithAI: "A tela de dashboard está sendo exibida corretamente"
```

## Combinando Assertions

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn: "Login"
- inputText: "usuario@email.com"
- tapOn: "Senha"
- inputText: "senha123"
- tapOn: "Entrar"

# Verifica que o login funcionou
- assertVisible: "Dashboard"
- assertNotVisible: "Tela de Login"
- assertNotVisible: "Erro de autenticação"
```

## Waiters (Espera por Condição)

O Maestro aguarda automaticamente elementos aparecerem antes de interagir. Mas você pode customizar:

```yaml
# Espera a animação de carregamento sumir
- waitForAnimationToEnd

# Espera explicitamente por um elemento com timeout maior
- assertVisible:
    text: "Dados carregados"
    timeout: 15000
```

## Timeout Padrão

Por padrão, o Maestro aguarda até **5 segundos** para elementos aparecerem. Para alterar globalmente, use o parâmetro `--timeout` no CLI:

```bash
maestro test flows/login.yaml --timeout 10000
```

---

> Ir para: [Primeiro Teste](4-Primeiro-teste.md)
