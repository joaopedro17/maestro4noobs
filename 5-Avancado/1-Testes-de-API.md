# Testes de API com Maestro

Combinar chamadas de API com testes de UI é uma estratégia poderosa: use a API para preparar o estado do app e a UI para verificar o resultado.

## Fazendo Chamadas HTTP com runScript

```javascript
// scripts/criar-usuario.js
var response = http.post("https://api.meuapp.com/usuarios", {
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    nome: "Usuário de Teste",
    email: "teste_" + Date.now() + "@email.com",
    senha: "Teste@123"
  })
});

var usuario = JSON.parse(response.body);
OUTPUT.email = usuario.email;
OUTPUT.senha = "Teste@123";
OUTPUT.userId = usuario.id;
```

```yaml
# flows/teste-com-api.yaml
appId: com.meuapp.android
---
# Cria o usuário via API antes do teste
- runScript: ../scripts/criar-usuario.js

# Agora testa a UI com o usuário criado
- launchApp:
    clearState: true

- tapOn: "E-mail"
- inputText: ${email}
- tapOn: "Senha"
- inputText: ${senha}
- tapOn: "Entrar"

- assertVisible: "Bem-vindo!"
```

## Verificando Dados via API Após Ação na UI

```javascript
// scripts/verificar-pedido.js
var response = http.get("https://api.meuapp.com/pedidos/" + maestro.env.PEDIDO_ID, {
  headers: { "Authorization": "Bearer " + maestro.env.TOKEN }
});

var pedido = JSON.parse(response.body);
OUTPUT.statusPedido = pedido.status;
```

```yaml
appId: com.loja.android
---
- launchApp
- runFlow: ../subflows/_login.yaml

# Ação na UI
- tapOn: "Confirmar Pedido"

# Captura o ID do pedido da tela
- evalScript: |
    var el = maestro.elementByText("Pedido #");
    OUTPUT.PEDIDO_ID = el.text.replace("Pedido #", "").trim();

# Verifica via API
- runScript: ../scripts/verificar-pedido.js

- assertTrue:
    condition: ${statusPedido == "CONFIRMADO"}
```

## Limpeza de Dados (Teardown)

```javascript
// scripts/deletar-usuario.js
http.delete("https://api.meuapp.com/usuarios/" + maestro.env.userId, {
  headers: { "Authorization": "Bearer " + maestro.env.ADMIN_TOKEN }
});
```

```yaml
appId: com.meuapp.android
---
- runScript: ../scripts/criar-usuario.js

# ... testes ...

# Limpa os dados após o teste
- runScript: ../scripts/deletar-usuario.js
```

## Boas Práticas

- Use a API para **setup** (criar dados) e **teardown** (limpar dados)
- Não teste lógica de negócio da API nos testes de UI — mantenha cada camada testando o que é dela
- Armazene tokens e credenciais em variáveis de ambiente, nunca hardcoded

---

> Ir para: [Testes Paralelos](2-Testes-paralelos.md)
