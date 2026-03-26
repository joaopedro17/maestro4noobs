# Dicas Gerais

## Maestro Studio

O **Maestro Studio** é a ferramenta mais poderosa para criar e depurar testes. É uma interface web interativa que mostra a tela do dispositivo em tempo real e permite inspecionar elementos.

Para abrir o Studio:

```bash
maestro studio
```

Uma janela do navegador abrirá com:

- **Espelho da tela** do dispositivo em tempo real
- **Árvore de hierarquia** de elementos
- **Console de comandos** para testar ações individualmente
- **Editor de flow** integrado

### Inspecionando elementos no Studio

1. Abra o Studio com `maestro studio`
2. Clique no elemento desejado na tela espelhada
3. Veja os identificadores disponíveis (id, texto, acessibilidade)
4. Copie o seletor recomendado para o seu flow

## Descobrindo o appId

Para encontrar o `appId` do app instalado no seu dispositivo:

**Android:**
```bash
adb shell pm list packages | grep nomedoapp
```

**iOS (Simulador):**
```bash
xcrun simctl listapps booted | grep nomedoapp
```

## Boas Práticas de Seletores

Use os identificadores na seguinte ordem de preferência:

1. **ID de acessibilidade** (`accessibilityIdentifier` no iOS, `contentDescription` no Android)
2. **Texto visível** — `tapOn: "Entrar"`
3. **ID de recurso** — `id: "btn_login"`
4. **Índice** — use apenas como último recurso: `tapOn:\n  index: 0`

## Usando o `.gitignore`

Crie um `.gitignore` no seu projeto:

```gitignore
.env
.env.*
!.env.example
*.log
.maestro/
```

## Executando em Modo Debug

Para ver logs detalhados durante a execução:

```bash
maestro test flows/login.yaml --format junit
```

Para salvar um relatório XML (compatível com CI):

```bash
maestro test flows/ --format junit --output resultado.xml
```

---

> Ir para: [Fluxos (Flows)](../3-Basico/1-Fluxos.md)
