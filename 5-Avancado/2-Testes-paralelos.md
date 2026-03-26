# Testes Paralelos

Executar testes em paralelo em múltiplos dispositivos reduz drasticamente o tempo total da suíte.

## Executando em Múltiplos Dispositivos

### Listando dispositivos disponíveis

```bash
# Android
adb devices

# iOS (simuladores)
xcrun simctl list devices
```

### Executando em um dispositivo específico

```bash
# Por ID do dispositivo Android
maestro --device emulator-5554 test flows/

# Por UDID do simulador iOS
maestro --device "iPhone 15 Pro (17.2)" test flows/
```

## Paralelismo com Script Shell

O Maestro não possui paralelismo nativo via CLI, mas você pode usar scripts para rodar em paralelo:

```bash
#!/bin/bash
# run-parallel.sh

# Divide os flows em grupos
FLOWS_1=(flows/autenticacao/login-sucesso.yaml flows/autenticacao/logout.yaml)
FLOWS_2=(flows/produtos/listar-produtos.yaml flows/produtos/buscar-produto.yaml)

# Executa em paralelo em dispositivos diferentes
maestro --device emulator-5554 test ${FLOWS_1[@]} &
maestro --device emulator-5556 test ${FLOWS_2[@]} &

# Aguarda todos terminarem
wait
echo "Todos os testes concluídos!"
```

## Paralelismo no GitHub Actions

```yaml
# .github/workflows/testes.yml
jobs:
  test:
    strategy:
      matrix:
        grupo: [autenticacao, produtos, checkout]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Executar testes - ${{ matrix.grupo }}
        run: maestro test flows/${{ matrix.grupo }}/
```

## Criando Múltiplos Emuladores

```bash
# Criar AVDs com nomes diferentes
avdmanager create avd -n "Pixel6_API34_1" -k "system-images;android-34;google_apis;x86_64"
avdmanager create avd -n "Pixel6_API34_2" -k "system-images;android-34;google_apis;x86_64"

# Iniciar em portas diferentes
emulator -avd Pixel6_API34_1 -port 5554 &
emulator -avd Pixel6_API34_2 -port 5556 &
```

## Boas Práticas

- Testes paralelos exigem **independência total** — cada teste deve criar e limpar seus próprios dados
- Use `clearState: true` para garantir estado limpo
- Prefira criar dados via API em vez de usar dados compartilhados

---

> Ir para: [CI/CD](3-CI-CD.md)
