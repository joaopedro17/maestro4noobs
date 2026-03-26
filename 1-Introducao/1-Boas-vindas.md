# Bem-vindo ao Maestro4Noobs!

## O que é o Maestro?

O **Maestro** é um framework de automação de testes para aplicativos mobile criado pela [Block](https://block.xyz/) (empresa por trás do Square e Cash App). Ele permite escrever testes de UI para iOS e Android usando arquivos **YAML simples**, sem necessidade de compilar código ou configurar ambientes complexos.

Com o Maestro você descreve o que o usuário faria na tela, e o framework executa essas ações no dispositivo real ou emulador.

## Por que usar o Maestro?

Testar aplicativos mobile sempre foi um desafio. Ferramentas como Appium, Espresso e XCUITest existem há anos, mas todas têm suas complexidades. O Maestro foi criado para ser **a forma mais simples de testar apps mobile**.

### Comparativo: Maestro vs Alternativas

| Critério | Maestro | Appium | Espresso | XCUITest |
|----------|---------|--------|----------|----------|
| Linguagem dos testes | YAML | JS/Python/Java/etc | Java/Kotlin | Swift/Objective-C |
| Suporte iOS + Android | ✅ | ✅ | ❌ só Android | ❌ só iOS |
| Configuração inicial | Muito simples | Complexa | Moderada | Moderada |
| Tempo de escrita de testes | Rápido | Moderado | Moderado | Moderado |
| Necessita compilar código | ❌ | ❌ | ✅ | ✅ |
| CI/CD | ✅ | ✅ | ✅ | ✅ |
| Gravação de testes (Studio) | ✅ | Parcial | ❌ | ❌ |

## Vantagens do Maestro

- **Sintaxe YAML simples**: qualquer pessoa consegue ler e entender um flow
- **Sem compilação**: edite o YAML e execute na hora
- **Maestro Studio**: interface visual para gravar e inspecionar testes
- **Funciona em iOS e Android**: mesma sintaxe para ambas as plataformas
- **Fluxos reutilizáveis**: subflows para evitar repetição de código
- **Integração com CI/CD**: roda em GitHub Actions, GitLab CI e outros
- **Maestro Cloud**: execute testes em infraestrutura na nuvem

## O que você vai aprender neste curso?

Ao longo deste guia você vai aprender a:

1. Instalar e configurar o Maestro no seu sistema operacional
2. Escrever flows YAML para interagir com apps mobile
3. Usar assertions para validar comportamentos
4. Organizar seu projeto com subflows e boas práticas
5. Integrar testes ao pipeline de CI/CD
6. Executar testes na nuvem com Maestro Cloud

## Exemplo rápido

Veja como é simples um teste de login com Maestro:

```yaml
appId: com.meuapp.android
---
- launchApp
- tapOn: "Campo de e-mail"
- inputText: "usuario@email.com"
- tapOn: "Senha"
- inputText: "minha-senha"
- tapOn: "Entrar"
- assertVisible: "Bem-vindo!"
```

Só isso! Sem importações, sem classes, sem métodos. Apenas YAML descrevendo o que o usuário faz.

---

> Ir para: [Comunicação](2-Comunicacao.md)
