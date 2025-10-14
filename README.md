# FIAP - Faculdade de Informática e Administração Paulista


<a href= "https://www.fiap.com.br/"><img width="2385" height="642" alt="logo-fiap" src="https://github.com/user-attachments/assets/62285a6c-34fe-4206-8a85-7ad584c6908b" alt="FIAP - Faculdade de Informática e Admnistração Paulista" border="0" width=40% height=40%></a>
</p>

<br>

# 📊 Fase 2 – 1TIAOS – Startup **FarmTech Solutions**  
## **Sistema Automatizado de Irrigação  FarmTech Solutions**

## 👨‍💻 Grupo 46

### 👨‍🎓 Integrantes:
- [Silvio Prestes Guerreiro Junior](https://www.linkedin.com/company/inova-fusca)

### 👩‍🏫 Professores:
- **Tutor(a):** [Sabrina Otoni](https://www.linkedin.com/company/inova-fusca)  
- **Coordenador(a):** [André Godoi Chiovato](https://www.linkedin.com/company/inova-fusca)

---

## 📡 Visão Geral do Projeto

Este projeto foi desenvolvido como parte da **Fase 2 do curso 1TIAO - FIAP**, no contexto da **Startup FarmTech Solutions**.  
O objetivo é simular um dispositivo eletrônico inteligente capaz de **monitorar variáveis ambientais agrícolas**, aplicando conceitos de **IoT (Internet das Coisas)** e automação com **ESP32**.

O sistema realiza a leitura de **temperatura, umidade, pH (simulado)** e **nutrientes (NPK)**, além de controlar um **relé** que representa o acionamento automático do sistema de irrigação.

---

## 🎥 Demonstração do Projeto

Assista ao vídeo demonstrativo com a solução em funcionamento completo no link abaixo:

🔗 [Clique aqui para assistir ao vídeo no YouTube](https://youtu.be/2VC28-53Fag)

---

## 🛠️ Estrutura do Projeto

O projeto foi desenvolvido no **Visual Studio Code** com **PlatformIO** e simulado na plataforma **Wokwi**.

```

📁 atividade1_Projeto_FarmTech
│
├── programa.ino               # Código principal do ESP32
├── platformio.ini             # Configuração do ambiente de compilação
├── wokwi.toml                 # Configuração da simulação no Wokwi
├── diagram.json               # Diagrama elétrico dos sensores e atuadores
└── README.md                  # Documentação do projeto

````

---

## 🧰 Componentes Utilizados (Simulados no Wokwi)

| Componente | Função | Pino ESP32 |
|------------|--------|------------|
| **DHT22** | Sensor de temperatura e umidade do ar | GPIO 15 |
| **Sensor de pH (simulado)** | Mede o nível de acidez do solo | GPIO 34 |
| **Botão Verde 1** | Simula sensor de Nitrogênio (N) | GPIO 18 |
| **Botão Verde 2** | Simula sensor de Fósforo (P) | GPIO 19 |
| **Botão Verde 3** | Simula sensor de Potássio (K) | GPIO 21 |
| **Módulo Relé** | Controla o sistema de irrigação | GPIO 23 |

> **Observação:**  
> O **sensor de pH é simulado pelo sensor LDR** apenas como representação analógica. Essa abordagem permite simular variações contínuas, semelhante a um sensor real.

---

## 🔄 Lógica e Funcionamento do Sistema

1. **Inicialização**  
   - Configura os pinos do ESP32.  
   - Inicia a comunicação serial e os sensores (DHT22 e pH).

2. **Leitura de Dados**  
   - Coleta temperatura e umidade.  
   - Lê o valor analógico do pH.  
   - Detecta o estado dos botões NPK.

3. **Processamento Lógico**  
   - Caso a umidade esteja baixa,  
   - Ou o pH esteja fora da faixa ideal,  
   - Ou haja deficiência de NPK,  
   → o sistema aciona o relé (irrigação **ON**).  
   Caso contrário, desliga o relé (**OFF**).

4. **Monitoramento Serial**  
   - Exibe os dados coletados no terminal serial.

5. **Loop Contínuo**  
   - A leitura é atualizada a cada **2 segundos**.

---

## 🔁 Fluxo de Operação

```
flowchart TD
    A[Início do Sistema] --> B[Inicializa componentes<br/> - GPIOs<br/> - Serial<br/> - DHT22 e pH]
    B --> C[Leitura dos Sensores<br/> - Temperatura e Umidade<br/> - pH do Solo<br/> - Botões NPK]
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

### 💻 Exemplo Simplificado de Lógica

```cpp
if (umidade < 40 || ph < 6.0 || botaoN == HIGH || botaoP == HIGH || botaoK == HIGH) {
    digitalWrite(rele, HIGH);   // Liga irrigação
} else {
    digitalWrite(rele, LOW);    // Desliga irrigação
}
```

---

## ⚙️ Configuração do Ambiente (PlatformIO)

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

## 🧪 Simulação no Wokwi

* **DHT22:** 3V3, GND e GPIO 15
* **Sensor de pH (LDR):** 3V3, GND e GPIO 34
* **Botões NPK:** GPIO 18, 19 e 21
* **Relé:** GPIO 23

Arquivo `wokwi.toml`:

```toml
[wokwi]
version = 1
elf = ".pio/build/esp32/firmware.elf"
firmware = ".pio/build/esp32/firmware.bin"
```

---

## 🗃 Histórico de lançamentos

* **0.6.0 - 14/10/2025**

  * Adição de vídeo demonstrativo da solução no YouTube.
  * Atualização da documentação e instruções no `README.md`.

* **0.5.0 - 13/10/2025**

  * Impressões do circuito adicionadas.
  * Inclusão de imagens da plataforma Wokwi para demonstração das conexões.

* **0.4.0 - 13/10/2025**

  * Criação e atualização do arquivo `README.md`.
  * Ajustes na estrutura do projeto e documentação.

* **0.3.0 - 07/10/2025**

  * Criação da pasta `atividade1` com sistema de gestão agrícola inicial.

* **0.2.0 - 07/10/2025**

  * Inclusão de arquivos adicionais para preparação do ambiente de desenvolvimento.

* **0.1.0 - 07/10/2025**

  * Commit inicial com criação do repositório e estrutura base do projeto.

---

## 📋 Licença

Este projeto foi desenvolvido exclusivamente para fins **acadêmicos** – **FIAP**.
Qualquer uso, modificação ou redistribuição deve seguir as diretrizes institucionais e de propriedade intelectual aplicáveis.
