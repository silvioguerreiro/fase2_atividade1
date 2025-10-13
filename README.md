##  Visão Geral do Projeto

Este projeto foi desenvolvido como parte da **Fase 2 do curso 1TIAO - FIAP**, no contexto da **Startup FarmTech Solutions**.  
O objetivo é simular um dispositivo eletrônico inteligente capaz de monitorar variáveis ambientais agrícolas, aplicando conceitos de IoT (Internet das Coisas) e automação com ESP32.

O sistema realiza a leitura de temperatura, umidade, pH (simulado) e nutrientes (NPK), e controla um relé que representa o acionamento automático de irrigação.

---

##  Estrutura do Projeto

O projeto foi desenvolvido no **Visual Studio Code** com **PlatformIO** e simulado na plataforma **Wokwi**.

```

📁 Projeto_FarmTech/
│
├── programa.ino               # Código principal do ESP32
├── platformio.ini             # Configuração do ambiente de compilação
├── wokwi.toml                 # Configuração da simulação no Wokwi
├── diagram.json               # Diagrama elétrico dos sensores e atuadores
└── README.md                  # Documentação do projeto

````

---

##  Componentes Utilizados (Simulados no Wokwi)

| Componente | Função | Pino ESP32 |
|-------------|--------|-------------|
|  **DHT22** | Sensor de temperatura e umidade do ar | GPIO 15 |
|  **Sensor de pH (simulado)** | Mede o nível de acidez do solo | GPIO 34 |
|  **Botão Verde 1** | Simula sensor de Nitrogênio (N) | GPIO 18 |
|  **Botão Verde 2** | Simula sensor de Fósforo (P) | GPIO 19 |
|  **Botão Verde 3** | Simula sensor de Potássio (K) | GPIO 21 |
|  **Módulo Relé** | Controla o sistema de irrigação | GPIO 23 |

>  **Observação:**  
> O **sensor de pH é simulado no circuito pelo sensor LDR**, apenas como uma representação analógica capaz de variar valores conforme estímulo.  
> Essa escolha permite simular leituras contínuas, como seria feito por um sensor de pH real em campo.

<img width="1920" height="1032" alt="circuito" src="https://github.com/user-attachments/assets/97df0ae0-2d01-4a18-ac21-5bd68746dd8d" />

---

##  Lógica e Funcionamento do Sistema

1. **Inicialização**  
   - Configura os pinos do ESP32;  
   - Inicia a comunicação serial e os sensores DHT22 e pH.

2. **Leitura de Dados**  
   - Coleta temperatura e umidade do DHT22;  
   - Lê o valor analógico do pH (simulado via LDR);  
   - Captura o estado dos botões NPK.

3. **Processamento Lógico**  
   - Se a Umidade estiver baixa,  
   - Ou o pH estiver fora da faixa ideal,  
   - Ou algum nutriente (NPK) indicar carência,  
   → o sistema aciona o relé (irrigação ON).  
   Caso contrário, o relé é desligado.

4. **Monitoramento Serial**  
   - Exibe no terminal os valores coletados e o status do sistema.

5. **Loop Contínuo**  
   - A leitura é atualizada a cada **2 segundos**.

---

##  Fluxo de Operação

```
flowchart TD
    A[Início do Sistema] --> B[Inicializa componentes<br/> - GPIOs<br/> - Serial<br/> - DHT22 e pH]
    B --> C[Leitura dos Sensores<br/> - Temperatura e Umidade<br/> - pH do Solo (LDR)<br/> - Botões NPK]
    C --> D[Processamento Lógico<br/>Verificação das condições de irrigação]
    D --> E{Umidade < limite<br/>ou<br/>pH fora da faixa<br/>ou<br/>NPK insuficiente?}
    E -->|Sim| F[Ativar Relé<br/>(Irrigação ON)]
    E -->|Não| G[Desligar Relé<br/>(Irrigação OFF)]
    F --> H[Enviar dados ao Serial Monitor]
    G --> H
    H --> I[Aguardar 2s]
    I --> C[Repetir ciclo]
````

---

###  Exemplo Simplificado de Lógica

```cpp
if (umidade < 40 || ph < 6.0 || botaoN == HIGH || botaoP == HIGH || botaoK == HIGH) {
    digitalWrite(rele, HIGH);   // Liga irrigação
} else {
    digitalWrite(rele, LOW);    // Desliga irrigação
}
```

<img width="1920" height="1032" alt="circuito em execução" src="https://github.com/user-attachments/assets/42a73ed1-d54a-447d-a32f-ae6954251e97" />

---

##  Configuração do Ambiente (PlatformIO)

```ini
[env:esp32]
platform = espressif32
framework = arduino
board = esp32dev

lib_deps =
    adafruit/Adafruit Unified Sensor@^1.1.9
    adafruit/DHT sensor library@1.4.4
```

---

##  Simulação no Wokwi

* **DHT22:** conectado aos pinos **3V3**, **GND** e **GPIO 15**.
* **Sensor de pH (simulado pelo LDR):** conectado ao **3V3**, **GND** e **GPIO 34**.
* **Botões NPK:** nos GPIOs **18**, **19** e **21**.
* **Relé:** acionado pelo **GPIO 23**.

Arquivo `wokwi.toml`:

```toml
[wokwi]
version = 1
elf = ".pio/build/esp32/firmware.elf"
firmware = ".pio/build/esp32/firmware.bin"
```

---

## Autor e Créditos

**Aluno:** Silvio Prestes Guerreiro Junior
**Turma:** 1TIAOS – Fase 2
**Instituição:** FIAP

* Visual Studio Code
* PlatformIO
* Wokwi Simulator
* Arduino Framework

---

## Referências

* FIAP (2025). *Um Mapa do Tesouro*
* FIAP (2025). *A Eletrônica de uma IA*
* Adafruit (2024). *DHT Sensor Library Documentation*
* Espressif Systems. *ESP32 Technical Reference Manual*
* FarmTech Solutions. *Projeto de Agricultura Digital e IoT*

---
