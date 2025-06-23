
# Atualização de Firmware e Testes com TL866 no Linux

Este guia explica como atualizar o firmware de gravadores TL866 (como TL866A/CS) no Linux, utilizando a ferramenta `libxgecu` e posteriormente fazer *dump* e visualizar os dados com `minipro`.

---

## ✅ Pré-requisitos

- Python 3.12 ou compatível
- `poetry` instalado
- Pacotes:
  ```bash
  sudo apt update
  sudo apt install git p7zip-full python3-pip
  ```

---

## 📦 Instalar o `libxgecu`

1. Clone o repositório:
   ```bash
   git clone https://github.com/JohnDMCmaster/libxgecu.git
   cd libxgecu
   ```

2. Instale o Poetry:
   ```bash
   curl -sSL https://install.python-poetry.org | python3 -
   export PATH="$HOME/.local/bin:$PATH"
   ```

3. Instale dependências e entre no ambiente virtual:
   ```bash
   poetry install
   poetry shell
   ```

---

## 🔄 Atualizar firmware

1. Copie o arquivo `updateT48.dat` desejado para o diretório atual.

2. Execute:
   ```bash
   poetry run t48_update updateT48.dat
   ```

   ⚠️ O gravador precisa estar conectado via USB.

---

## 🔍 Testar o funcionamento com `minipro`

### Instalar:

```bash
sudo apt install minipro
```

### Fazer dump de um chip (exemplo: 24c02):

```bash
minipro -p "AT24C02" -r dump.hex
```

---

## 👁️ Visualizar arquivo HEX

Você pode abrir o `dump.hex` com editores como:

- `xxd` (terminal):
  ```bash
  xxd dump.hex | less
  ```

- `GHex` (interface gráfica):
  ```bash
  sudo apt install ghex
  ghex dump.hex
  ```

---

## ✅ Verificar versão do firmware

Dentro do ambiente virtual `poetry`:
```bash
poetry run t48_version
```

---

## ℹ️ Dicas

- Use `find` para localizar firmwares:
  ```bash
  find . -iname "*updateT48.dat*"
  ```

- Se necessário, extraia `.exe` do XGPro com:
  ```bash
  7z x XgproV1255_Setup.exe -oXgproV1255
  ```

---

## 🧩 Referências

- [libxgecu no GitHub](https://github.com/JohnDMCmaster/libxgecu)
- [minipro no Linux](https://gitlab.com/DavidGriffith/minipro)

---

## ⚠️ Compatibilidade de Firmware

Para que a gravação ou leitura funcione corretamente, **o firmware do gravador precisa estar na versão `01.1.32 (0x120)`**.

Se você estiver com uma versão superior e encontrar erros, **é necessário realizar um downgrade** do firmware.

### Como fazer downgrade

1. Encontre o arquivo `updateT48.dat` correspondente à versão `01.1.32`. Esse arquivo pode ser extraído de versões antigas do XGPro usando o `7z`:

   ```bash
   7z x XgproV1132_Setup.exe -oXgproV1132
   ```

2. Em seguida, utilize o comando:

   ```bash
   poetry run t48_update updateT48.dat
   ```

   Certifique-se de que o gravador está conectado via USB.

3. Confirme a versão do firmware após o procedimento com:

   ```bash
   poetry run t48_version
   ```

Se a versão correta estiver ativa, o `minipro` e outras ferramentas compatíveis funcionarão normalmente.
