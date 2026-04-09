# Documentação Técnica: CPU de 8 Bits

## 1. Introdução
Esta CPU de 8 bits foi desenvolvida no simulador Digital para fins de estudo de arquitetura de computadores. Diferente de sistemas complexos de múltiplos registradores, este projeto implementa uma Arquitetura de Acumulador. Nela, o resultado de toda operação aritmética ou lógica é armazenado em um único registrador central (o Acumulador), que serve como uma das entradas para a operação seguinte.

## 2. Componentes e Construção

### 2.1 Unidade de Memória (EEPROM)

A EEPROM atua como a memória de programa, armazenando as instruções que a CPU deve executar.

Entrada A (Address): Recebe o endereço de memória (geralmente vindo de um Program Counter externo).

Saída D (Data): Uma palavra de dados que é dividida por um Splitter para controle do sistema:

Bits 0-7 (Operando): Fornece o valor imediato (Dado) para a entrada N da ALU.

Bits 8-10 (OpCode): Define a operação a ser realizada através do pino Sel.

### 2.2 Unidade Lógica e Aritmética (ALU)

A ALU é o núcleo de processamento do sistema. Ela foi projetada para operar de forma modular:

Entrada ACin: Recebe o valor atual armazenado no Registrador (feedback).

Entrada N: Recebe o valor vindo diretamente da instrução (EEPROM).

Entrada Sel: Recebe o código da operação (ex: Soma, Subtração, AND, etc.).

Saída AC: Envia o resultado do cálculo para a entrada do registrador.

### 2.3 Registrador de Acumulador (Reg)

O registrador funciona como a memória de estado da CPU, estabilizando o resultado da ALU.

Sincronização: Utiliza o sinal de Clock para atualizar seu valor apenas no momento correto do ciclo.

Habilitação (en): Controlado por um sinal lógico que permite ou impede a escrita do novo dado.

Saída Q: Alimenta simultaneamente os displays de saída e retorna para a entrada da ALU, fechando o ciclo de acumulação.

## 3. Arquitetura e Integração do Sistema

Diferente de uma arquitetura de pilha, onde os dados são empilhados na RAM, este sistema utiliza um Loop de Retroalimentação (Feedback Loop) direto:O Fluxo de Dados: O dado processado pela ALU é salvo no Registrador.Feedback: A saída do Registrador é ligada diretamente de volta à entrada ACin da ALU. Isso permite realizar operações cumulativas (Ex: $Total = Total + Valor$).Exibição Visual: O barramento de saída do registrador é conectado a dois Displays de 7 Segmentos, permitindo monitorar o valor binário convertido em representação visual em tempo real.

## 4. Funcionamento Passo a Passo

O ciclo de execução da CPU segue estas etapas:

Busca (Fetch): O pulso de Clock aciona a EEPROM, que disponibiliza a instrução baseada no endereço atual.

Decodificação: O Splitter separa o valor numérico (N) do comando de operação (Sel).

Execução: A ALU processa o valor vindo do registrador com o valor vindo da memória.

Escrita (Store): No próximo ciclo, o resultado da ALU é capturado pelo registrador, atualizando o display e preparando a CPU para a próxima instrução.

Vídeo explicativo: 