## Visão Geral
Este código em Assembly para um microcontrolador AVR implementa um sistema de aquisição de dados que lê valores de um conversor analógico-digital (ADC), calcula a média de múltiplas leituras e exibe os resultados em formato hexadecimal e decimal via UART (Universal Asynchronous Receiver/Transmitter). O sistema também incorpora um atraso configurável para estabilização das leituras.

## Requisitos de Hardware
- Microcontrolador AVR (ex: ATmega328P)
- Sensor ou dispositivo que forneça uma saída analógica para o ADC
- Conexões UART para transmissão de dados (ex: módulo Bluetooth ou cabo serial)
- Ambiente de desenvolvimento AVR para compilação (ex: Atmel Studio)
- Um clock externo rodando a 16 MHz (não especificado, mas comumente utilizado)

## Funcionalidades
- **Leitura do ADC**: O código realiza leituras do ADC e armazena os resultados para cálculos posteriores.
- **Cálculo da Média**: A média de 8 leituras consecutivas é calculada e convertida em formato hexadecimal e decimal.
- **Transmissão Serial**: Os resultados são enviados via UART, permitindo a visualização em um monitor serial.
- **Controle de Atraso**: O sistema incorpora um atraso entre as leituras para garantir a estabilidade dos dados coletados.

## Estrutura do Código

### Definições e Inicializações
- `ATRASO`: Define um valor de atraso, usado para estabilizar as leituras do ADC.
- **Reset**: Inicializa o ponteiro da pilha (Stack Pointer) para o final da memória RAM.

### Inicialização da UART (`INIT_SERIAL`)
- Configura os registradores da UART para habilitar a comunicação serial:
  - `UCSR0A`, `UCSR0B`, `UCSR0C` são configurados para definir os parâmetros de transmissão.
  - O valor `207` é configurado para o registrador `UBRR0` para definir a taxa de transmissão.

### Inicialização do ADC (`INIT_ADC`)
- Configura os registradores `ADCSRA` e `ADMUX` para habilitar o ADC e selecionar a referência de tensão.

### Loop Principal (`MAIN`)
- Um loop que realiza as seguintes tarefas:
  1. Reseta os registradores utilizados para o cálculo da média.
  2. Realiza um loop de 8 iterações para coletar leituras do ADC.
  3. Calcula a média das leituras.
  4. Converte os valores em formato hexadecimal e decimal.
  5. Transmite os resultados via UART.
  6. Implementa uma pausa entre as leituras.

### Leitura do ADC (`READ_ADC`)
- Inicia a conversão do ADC e espera até que a conversão esteja completa. Armazena os valores lidos em registradores.

### Funções de Conversão
- **`U16_TO_DEC`**: Converte um número de 16 bits em seu formato decimal.
- **`U8_TO_HEX`**: Converte um número de 8 bits em seu formato hexadecimal.
- **`U4_TO_HEX`**: Converte um número de 4 bits em seu formato hexadecimal.

### Transmissão Serial (`TX`)
- Envia dados via UART, aguardando até que o registrador esteja pronto para a próxima transmissão.

### Atraso (`DELAY`)
- Implementa uma função de atraso baseada em loops, garantindo que o tempo especificado seja respeitado.

## Como Funciona
1. **Inicialização**: O microcontrolador é inicializado configurando a UART e o ADC.
2. **Coleta de Dados**: Um loop de leitura realiza 8 coletas do ADC, somando os resultados para calcular a média.
3. **Transmissão dos Resultados**: Os resultados são formatados em hexadecimal e decimal e enviados pela UART.
4. **Ciclo Repetido**: O processo é repetido continuamente, permitindo a aquisição e transmissão contínuas de dados.

## Compilação e Uso
- Para compilar e carregar o código, utilize um conjunto de ferramentas compatível com AVR, como AVR-GCC, e um programador (ex: USBasp).
- Conecte o microcontrolador ao sensor analógico e ao dispositivo de comunicação UART.
- Após programar o microcontrolador, os dados começarão a ser coletados e transmitidos automaticamente, podendo ser visualizados em um monitor serial.

## Notas
- Certifique-se de que a configuração do clock do microcontrolador esteja correta para a taxa de transmissão da UART.
- O código assume que os dados recebidos do ADC estão dentro dos limites esperados; ajustes podem ser necessários dependendo do sensor utilizado.
