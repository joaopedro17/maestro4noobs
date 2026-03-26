# Subflows

Subflows são flows reutilizáveis que podem ser chamados por outros flows. São essenciais para evitar repetição e manter o projeto organizado.

## Criando um Subflow

```yaml
# subflows/_login.yaml
# Este subflow não tem appId — é executado no contexto do flow pai

- tapOn: "E-mail"
- inputText: ${EMAIL}
- tapOn: "Senha"
- inputText: ${SENHA}
- tapOn: "Entrar"
```

## Chamando um Subflow

```yaml
# flows/meu-teste.yaml
appId: com.meuapp.android
---
- launchApp:
    clearState: true

# Chama o subflow
- runFlow: ../subflows/_login.yaml

# Continua o teste após o login
- assertVisible: "Dashboard"
- tapOn: "Perfil"
```

## Passando Variáveis para Subflows

```yaml
# flows/login-usuario-admin.yaml
appId: com.meuapp.android
---
- launchApp:
    clearState: true

- runFlow:
    file: ../subflows/_login.yaml
    env:
      EMAIL: "admin@empresa.com"
      SENHA: "AdminSenha@123"

- assertVisible: "Painel Administrativo"
```

## Exemplo Completo: E-commerce

### Subflow de Login

```yaml
# subflows/_login.yaml
- tapOn: "Entrar"
- tapOn: "E-mail"
- inputText: ${EMAIL}
- tapOn: "Senha"
- inputText: ${SENHA}
- tapOn: "Confirmar"
```

### Subflow de Adicionar ao Carrinho

```yaml
# subflows/_adicionar-carrinho.yaml
- scrollUntilVisible:
    element:
      text: ${PRODUTO}
    direction: DOWN
- tapOn: ${PRODUTO}
- tapOn: "Adicionar ao Carrinho"
- assertVisible: "Adicionado!"
```

### Flow Principal: Compra Completa

```yaml
# flows/compra-completa.yaml
appId: com.loja.android
---
- launchApp:
    clearState: true

- runFlow:
    file: ../subflows/_login.yaml
    env:
      EMAIL: "comprador@email.com"
      SENHA: "Senha@123"

- runFlow:
    file: ../subflows/_adicionar-carrinho.yaml
    env:
      PRODUTO: "Tênis Nike Air Max"

- tapOn: "Carrinho"
- tapOn: "Finalizar Compra"
- assertVisible: "Pedido realizado com sucesso!"
```

## Boas Práticas

- Prefixe subflows com `_` para diferenciar de flows principais
- Mantenha subflows genéricos usando variáveis (`${VARIAVEL}`)
- Subflows não devem ter `appId` — herdam do flow pai
- Nomeie subflows pelo que fazem: `_login.yaml`, `_add-to-cart.yaml`

---

> Ir para: [Condições](2-Condicoes.md)
