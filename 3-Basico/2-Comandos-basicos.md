# Comandos Básicos

O Maestro oferece uma variedade de comandos para interagir com aplicativos mobile. Veja os mais utilizados:

## Gerenciamento do App

```yaml
# Abre o app (mantém estado)
- launchApp

# Abre o app e limpa o estado (dados, login, etc.)
- launchApp:
    clearState: true

# Fecha o app sem limpar dados
- stopApp

# Limpa os dados do app
- clearState
```

## Toque e Clique

```yaml
# Toque por texto visível
- tapOn: "Entrar"

# Toque por ID de acessibilidade
- tapOn:
    id: "btn_login"

# Toque por índice (quando há múltiplos elementos iguais)
- tapOn:
    text: "Item"
    index: 2

# Toque longo
- longPressOn: "Produto"

# Duplo toque
- doubleTapOn: "Foto"

# Toque em coordenadas específicas (evite quando possível)
- tapOn:
    point: "50%, 50%"
```

## Entrada de Texto

```yaml
# Digita texto no campo focado
- inputText: "meu texto aqui"

# Limpa o campo focado
- clearText

# Pressiona uma tecla específica
- pressKey: Enter
- pressKey: Backspace
- pressKey: Back  # botão voltar do Android
```

## Rolagem e Swipe

```yaml
# Rola para baixo
- scroll

# Rola até um elemento ficar visível
- scrollUntilVisible:
    element:
      text: "Item no fundo"
    direction: DOWN

# Swipe em direção específica
- swipe:
    direction: LEFT

# Swipe de coordenada para coordenada
- swipe:
    startX: 80%
    startY: 50%
    endX: 20%
    endY: 50%
```

## Navegação

```yaml
# Abre uma deep link ou URL
- openLink: "meuapp://tela-produto/123"

# Volta para a tela anterior (Android)
- pressKey: Back

# Vai para a tela inicial
- pressKey: Home
```

## Espera

```yaml
# Espera um elemento aparecer (timeout padrão: 5s)
- waitForAnimationToEnd

# Espera um número de milissegundos
- wait:
    ms: 2000

# Espera um elemento ficar visível (com timeout customizado)
- assertVisible:
    text: "Carregando..."
    timeout: 10000
```

## Boas Práticas

- Prefira selecionar por **texto visível** ou **ID de acessibilidade**
- Evite usar `point` com coordenadas — quebra em diferentes tamanhos de tela
- Use `clearState: true` quando o teste precisa começar do zero
- Prefira `scrollUntilVisible` ao invés de swipes manuais

---

> Ir para: [Assertions](3-Assertions.md)
