# Condições

O Maestro permite execução condicional de passos usando o campo `when`. Isso é útil para lidar com comportamentos diferentes entre plataformas ou estados do app.

## Condição por Visibilidade

```yaml
appId: com.meuapp.android
---
- launchApp

# Executa o passo SOMENTE se o elemento estiver visível
- tapOn: "Aceitar Cookies"
  when:
    visible: "Aceitar Cookies"

# Assim você lida com pop-ups opcionais sem falhar o teste
- assertVisible: "Tela Principal"
```

## Condição por Plataforma

```yaml
appId: com.meuapp
---
- launchApp

# Executa apenas no Android
- tapOn: "Botão Android"
  when:
    platform: Android

# Executa apenas no iOS
- tapOn: "Botão iOS"
  when:
    platform: iOS
```

## Condição com JavaScript

Para condições mais complexas, use expressões JavaScript:

```yaml
appId: com.meuapp.android
---
- launchApp

# Executa somente se a variável estiver definida
- runFlow: ../subflows/_login.yaml
  when:
    true: ${EXECUTAR_LOGIN == 'true'}
```

## Condicionais com runFlow

```yaml
appId: com.meuapp.android
---
- launchApp

# Se o pop-up de avaliação aparecer, fecha ele
- runFlow:
    file: ../subflows/_fechar-popup.yaml
    when:
      visible: "Avalie nosso app"

# Continua o fluxo principal
- tapOn: "Produtos"
```

## Exemplo: Onboarding Opcional

Alguns apps exibem uma tela de onboarding apenas na primeira execução. Com condicionais, você lida com isso graciosamente:

```yaml
appId: com.meuapp.android
---
- launchApp

# Se a tela de onboarding aparecer, pula ela
- tapOn: "Pular"
  when:
    visible: "Pular"

# Se tiver um pop-up de permissão de notificação
- tapOn: "Permitir"
  when:
    visible: "Permitir Notificações"

# Agora o teste pode continuar de forma confiável
- assertVisible: "Tela Principal"
- tapOn: "Entrar"
```

## Boas Práticas

- Use condicionais para elementos **opcionais** como pop-ups e banners
- Não abuse de condicionais para cobrir fluxos principais — use flows separados
- `when: visible` é mais confiável que condições JavaScript para a maioria dos casos

---

> Ir para: [JavaScript](3-JavaScript.md)
