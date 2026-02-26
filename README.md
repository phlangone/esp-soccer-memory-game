# Jogo da Memória: Times de Futebol com Múltiplos Displays OLED

## Descrição do Projeto
Este projeto consiste em um jogo da memória físico interativo, desenvolvido em C/C++ para microcontroladores. O sistema utiliza 8 displays OLED individuais para exibir emblemas de times de futebol de diversos torneios mundiais. 

O jogador seleciona um torneio, memoriza a posição dos emblemas exibidos temporariamente nos 8 displays e, em seguida, utiliza botões físicos dedicados para tentar encontrar os pares correspondentes. O jogo conta com feedback sonoro (buzzer) para acertos, erros e uma melodia de vitória ao finalizar a partida.

**Autoria:** Paulo H. Langone - SENAI Anchieta (CST Eletrônica Industrial - EXTUN4)

## Funcionalidades
* **Menu de Seleção de Torneios:** 8 opções de ligas (Brasileirão, Bundesliga, Copa do Mundo, La Liga, MLS, Premier League, Serie A, UAFA).
* **Embaralhamento Aleatório:** Algoritmo que garante que as posições dos times sejam diferentes a cada nova rodada.
* **Multiplexação de Displays:** Controle de 8 displays I2C de mesmo endereço (0x3C) através de seleção de hardware (multiplexador demultiplexador no barramento SDA/SCL).
* **Feedback Audiovisual:** Sinais sonoros distintos para jogadas corretas e incorretas, além de animação e música ao concluir o jogo.

## Hardware Necessário
* 1x Microcontrolador (Ex: ESP32)
* 8x Displays OLED SSD1306 (128x64 pixels, interface I2C)
* 8x Botões (Push-buttons)
* 1x Buzzer
* 1x Multiplexador (Ex: CD74HC4051 ou similar) para gerenciar a comunicação I2C dos displays.

### Mapeamento de Pinos (Pinout)
| Componente | Pino no Microcontrolador |
| :--- | :--- |
| **Buzzer** | GPIO 16 |
| **Botão 0** | GPIO 13 |
| **Botão 1** | GPIO 12 |
| **Botão 2** | GPIO 14 |
| **Botão 3** | GPIO 27 |
| **Botão 4** | GPIO 26 |
| **Botão 5** | GPIO 25 |
| **Botão 6** | GPIO 33 |
| **Botão 7** | GPIO 32 |
| **Mux Seleção A** | GPIO 15 |
| **Mux Seleção B** | GPIO 2 |
| **Mux Seleção C** | GPIO 5 |

## Dependências e Bibliotecas
Para compilar e executar este código, é necessário instalar as seguintes bibliotecas na sua IDE:
* `Wire.h` (Nativa)
* `Adafruit_GFX.h`
* `Adafruit_SSD1306.h`

**Nota:** O projeto depende de um arquivo local chamado `teams.h`. Este arquivo deve conter os arrays de bytes (bitmaps) em formato hexadecimal para a renderização dos logotipos dos times e dos torneios na tela OLED.

## Lógica de Funcionamento
1. **Inicialização:** Os pinos são configurados e os displays são inicializados através da varredura do multiplexador.
2. **Seleção de Torneio:** Cada display mostra o logo de um torneio. O jogador pressiona o botão correspondente ao torneio desejado.
3. **Memorização:** O sistema sorteia as posições, embaralha os times do torneio escolhido e os exibe por 2 segundos (`TEMPO_EXIBICAO`).
4. **Loop de Jogo:** As telas são apagadas. O sistema aguarda o jogador pressionar dois botões.
   * Se os times coincidirem: Os displays correspondentes são desativados desta rodada e um som de acerto é emitido.
   * Se os times não coincidirem: Um som de erro é emitido e os displays são apagados novamente.
5. **Fim de Jogo:** Ao acertar os 4 pares, uma imagem de comemoração é exibida simultaneamente nos 8 displays, acompanhada de uma melodia programada.
