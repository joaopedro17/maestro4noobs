# JavaScript nos Flows

O Maestro permite executar JavaScript dentro dos flows com o comando `evalScript` e `runScript`. Isso possibilita lógica dinâmica, cálculos e integrações com APIs externas.

## evalScript — Executando Scripts Inline

```yaml
appId: com.meuapp.android
---
- launchApp

# Define uma variável via JavaScript
- evalScript: ${OUTPUT.timestamp = new Date().toISOString()}

# Usa a variável no flow
- tapOn: "Campo de Data"
- inputText: ${timestamp}
```

## Extraindo Texto da Tela

O Maestro expõe a hierarquia da tela em JavaScript via `maestro`:

```yaml
appId: com.meuapp.android
---
- launchApp
- assertVisible: "Total: R$"

# Extrai o texto do elemento e salva em uma variável
- evalScript: |
    var elemento = maestro.elementByText("Total:");
    OUTPUT.valorTotal = elemento ? elemento.text : "0";

- assertTrue:
    condition: ${valorTotal.includes("R$")}
```

## runScript — Executando Arquivos JavaScript

Para scripts maiores, crie um arquivo `.js` separado:

```javascript
// scripts/calcular-desconto.js
var preco = parseFloat(maestro.elementByText("Preço:").text.replace("R$ ", ""));
var desconto = preco * 0.1;
OUTPUT.precoComDesconto = (preco - desconto).toFixed(2);
```

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn: "Produto"

- runScript: ../scripts/calcular-desconto.js

- evalScript: ${OUTPUT.precoFinal = "R$ " + precoComDesconto}
- assertVisible: ${precoFinal}
```

## Chamadas HTTP com JavaScript

Você pode fazer chamadas de API dentro de scripts:

```javascript
// scripts/criar-usuario-teste.js
var response = http.get("https://api.meuapp.com/usuarios/aleatorio");
var usuario = JSON.parse(response.body);
OUTPUT.email = usuario.email;
OUTPUT.senha = usuario.senha;
```

```yaml
appId: com.meuapp.android
---
- runScript: ../scripts/criar-usuario-teste.js

- launchApp:
    clearState: true

- tapOn: "E-mail"
- inputText: ${email}
- tapOn: "Senha"
- inputText: ${senha}
- tapOn: "Entrar"
```

## Objetos Disponíveis no JavaScript

| Objeto | Descrição |
|--------|-----------|
| `maestro` | Acesso à hierarquia da tela |
| `http` | Fazer requisições HTTP |
| `OUTPUT` | Variáveis de saída para o flow |
| `console.log()` | Log para debugging |

## Boas Práticas

- Use JavaScript com moderação — YAML simples é mais legível
- Prefira `evalScript` para expressões simples
- Use `runScript` com arquivo separado para lógica complexa
- Documente o propósito de scripts não óbvios

---

> Ir para: [Variáveis de Ambiente](4-Variaveis-de-ambiente.md)
