# Sistema de Autenticação RFID com ESP32

Sistema de controle de acesso utilizando leitor RFID MFRC522 e ESP32, com autenticação via API REST.

## 📋 Requisitos

### Hardware
- ESP32 DevKit V1
- Leitor RFID MFRC522
- 2 LEDs (vermelho e verde)
- Resistores apropriados para os LEDs
- Cartões/tags RFID

### Software
- [PlatformIO](https://platformio.org/) - IDE extension para VS Code
- [API](https://github.com/ivtrL/apiESP32Project)

## 🚀 Instalação

### 1. Instalar PlatformIO

#### Via VS Code (Recomendado)
1. Abra o Visual Studio Code
2. Vá em **Extensions** (Ctrl+Shift+X)
3. Procure por "PlatformIO IDE"
4. Clique em **Install**
5. Reinicie o VS Code após a instalação

#### Via CLI
```bash
pip install -U platformio
```

### 2. Importar o Projeto

#### Método 1: Clonar o repositório
```bash
git clone https://github.com/ivtrL/RFIDAttendance.git
cd RFIDAttendance
code .
```

#### Método 2: Abrir projeto existente
1. Abra o VS Code
2. Clique no ícone do PlatformIO na barra lateral (alien icon)
3. Selecione **Open Project**
4. Navegue até a pasta do projeto e selecione

### 3. Configurar o Projeto

Edite o arquivo `src/main.cpp` e configure as seguintes variáveis:

```cpp
// Credenciais WiFi
const char *ssid = "WIFI-NAME";              // Nome da sua rede WiFi
const char *passwordWifi = "WIFI-PASSWORD";  // Senha do WiFi

// Endpoints da API
char httpLoginServer[] = "API-DOMAIN-WEBSITE/api/device/login";
char httpRefreshTokenServer[] = "API-DOMAIN-WEBSITE/api/auth/refresh-token/device";
char httpCheckCardServer[] = "API-DOMAIN-WEBSITE/api/card/check";

// Credenciais do dispositivo
authLoginRequest.email = "teste@gmail.com";
authLoginRequest.password = "teste123";
authLoginRequest.deviceName = "TESTEAPI";
authLoginRequest.deviceUid = "8b7bd2787758f8f2f922c51d8fcfeb86";
```

### 4. Conexões do Hardware

| MFRC522 | ESP32 |
|---------|-------|
| SDA/SS  | GPIO 5 |
| SCK     | GPIO 18 |
| MOSI    | GPIO 23 |
| MISO    | GPIO 19 |
| RST     | GPIO 2 |
| 3.3V    | 3.3V |
| GND     | GND |

| LED       | ESP32 |
|-----------|-------|
| LED Verde | GPIO 4 |
| LED Vermelho | GPIO 21 |

## 🔧 Compilar e Upload

### Via Interface do PlatformIO
1. Conecte o ESP32 via USB
2. Na barra inferior do VS Code, clique no ícone **Upload** (→)
3. Aguarde a compilação e upload

### Via Terminal
```bash
# Compilar
pio run

# Upload
pio run --target upload

# Monitor Serial
pio device monitor
```

### Porta Serial
O projeto está configurado para usar `COM3`. Para alterar:
1. Abra `platformio.ini`
2. Modifique a linha `upload_port = COM3` para sua porta (ex: `COM4`, `/dev/ttyUSB0`)

## 📡 Endpoints HTTP

O dispositivo cria um servidor web com os seguintes endpoints:

- `GET /open` - Simula abertura (pisca LED verde 5 vezes)
- `GET /closed` - Simula bloqueio (pisca LED vermelho 5 vezes)

## 📊 Funcionamento

1. **Inicialização**: O dispositivo conecta ao WiFi e faz login na API
2. **Loop Principal**: Aguarda aproximação de cartão RFID
3. **Verificação**: Quando detecta um cartão, envia o UID para a API
4. **Resposta**: 
   - ✅ **Autorizado**: LED verde pisca 5 vezes
   - ❌ **Bloqueado**: LED vermelho pisca 5 vezes

## 🐛 Troubleshooting

### Erro ao conectar WiFi
- Verifique o SSID e senha
- Certifique-se que a rede é 2.4GHz (ESP32 não suporta 5GHz)

### Erro ao fazer upload
- Verifique a porta serial correta
- Pressione o botão BOOT no ESP32 durante o upload se necessário

### Leitor RFID não detecta cartões
- Verifique as conexões SPI
- Teste com diferentes cartões/tags
- Verifique alimentação de 3.3V

## 📚 Bibliotecas Utilizadas

- ArduinoJson v6.21.3
- MFRC522 v1.4.10
- ESPAsyncWebServer-esphome v3.2.2
