<div align="center" style="padding-bottom: 80px;">
<img align="right" alt="logo dimap" height="60" src="docs/assets/dimap.png">
<img align="left" alt="logo ufrn" height="60" src="docs/assets/ufrn-logo.png">
</div>

<div align="center">

# 🏢 Sistema de Controle de Elevadores em VHDL

</div>

## 📝 Sobre o projeto

Sistema completo para controle de **três elevadores** em um edifício de **32 andares (0 a 31)**, implementado em **VHDL**. O projeto utiliza uma arquitetura hierárquica de dois níveis com Máquinas de Estados Finitos (FSMs) para gerenciamento robusto e eficiente de múltiplos elevadores.

Desenvolvido como projeto final da disciplina **Circuitos Digitais** (UFRN), demonstrando conceitos avançados de:
- Arquitetura hierárquica multinível
- Máquinas de Estados Finitos (FSM)
- Algoritmos de escalonamento
- Sistemas de controle em tempo real
- Design modular em VHDL
- Simulação e validação com testbenches

**Professor:** Márcio Eduardo Kreutz  
**Data:** Outubro de 2025

> 📄 Para mais detalhes, acesse o [relatório completo](docs/report/Trabalho_Circuitos.pdf).

## 👥 Colaboradores

- [Luisa Ferreira](https://github.com/luisaferreirass)
- [Kauã do Vale Ferreira](https://github.com/DevlTz)
- [Cícero Paulino de Oliveira Filho](https://github.com/ciceropaulino)
- [Ryan David dos Santos Silvestre](https://github.com/RyanSilv257)

## 🎯 Objetivos

O objetivo principal é desenvolver um sistema robusto e eficiente para controlar a operação de múltiplos elevadores. A arquitetura é rigidamente dividida para garantir modularidade e clareza:

1. **Nível 1 (Controlador Local - 3x):** Gerencia as operações físicas de cada elevador individual (motor, portas, registro de andar)
2. **Nível 2 (Escalonador/Supervisor - 1x):** Gerencia todas as chamadas externas, decide qual dos três elevadores deve atender cada requisição e distribui comandos para os controladores locais

Todo o projeto é validado através de simulação com testbenches em VHDL.

## 🏗️ Arquitetura Geral

A arquitetura segue o modelo de controle hierárquico de dois níveis:

![Diagrama de blocos do sistema de controle de três elevadores](docs/diagramas_projeto/diagrama2710.png)

### Módulos Principais

| Módulo | Função | Lógica Principal |
|--------|--------|------------------|
| **Escalonador (Supervisor)** | Gerencia chamadas externas, aplica o algoritmo de escalonamento e distribui requisições aos elevadores | Lógica de seleção e FSM de alto nível |
| **Controlador Local (3x)** | Coordena a operação de cada elevador (motor e porta). Recebe chamadas internas e requisições do Escalonador | FSM principal com estados: `IDLE`, `MOVENDO`, `ABRINDO_PORTA`, `PORTA_ABERTA`, `FECHANDO_PORTA` |
| **Componentes (Motor, Porta)** | Componentes "executores" que obedecem aos comandos dos Controladores Locais | FSM para o Motor (`PARADO`, `SUBINDO`, `DESCENDO`, `FREANDO`) e lógica sequencial para a Porta |

## 🔌 Interface de Sinais (Top-Level)

O módulo top-level é o ponto de conexão de todos os blocos do sistema.

### Entradas Globais (do Testbench)

- `clk`, `rst`: Clock global e reset
- `chamadas_externas_subir [0..30]`: Botões de subida dos andares
- `chamadas_externas_descer [1..31]`: Botões de descida dos andares
- `chamadas_internas_E1, E2, E3 [0..31]`: Painéis internos de cada elevador
- `sensor_andar_E1, E2, E3 [0..31]`: Sensores de posição (simulados)

### Saídas Globais (para Displays/Testbench)

- `display_andar_E1, E2, E3`: Indicador de andar atual
- `display_porta_E1, E2, E3`: Indicador de porta (aberta ou fechada)
- `display_direcao_E1, E2, E3`: Indicador de movimento (subindo ou descendo)

## 📊 Estratégia de Escalonamento

O projeto utiliza primariamente o método **Scan (Elevador Varredura)**.

### Método Scan

Consiste em percorrer todos os andares em um único sentido (para cima ou para baixo), atendendo todas as requisições no caminho. A direção só é invertida ao atingir os limites do edifício (térreo ou cobertura) ou a última requisição pendente naquela direção.

**Vantagens:**
- Implementação simples e clara
- Comportamento previsível
- Adequado para implementação estrutural em VHDL

### Alternativas Consideradas

Foram avaliadas as seguintes abordagens:

- **Scan básico** (escolhida): Simplicidade de implementação
- **Proximidade e Precedência**: Embora mais eficiente em termos de tempo de espera, foi considerada excessivamente complexa para a implementação estrutural em VHDL do projeto

## ⚙️ Parâmetros do Sistema

| Parâmetro | Valor | Notas |
|-----------|-------|-------|
| **Número de Andares** | 32 (0 a 31) | Térreo a cobertura |
| **Número de Elevadores** | 3 | Operação simultânea |
| **Timer da Porta** | 250.000.000 ciclos | Equivalente a 5 segundos com clock de 50MHz |
| **Clock do Sistema** | 50 MHz | Frequência padrão |
| **Tempos de Operação** | Variáveis | Definidos nos módulos Motor e Porta |

## 🎮 Máquinas de Estados

### Controlador Local (Elevador)
```
IDLE → MOVENDO → FREANDO → ABRINDO_PORTA → PORTA_ABERTA → FECHANDO_PORTA → IDLE
```

### Motor
```
PARADO ↔ SUBINDO
       ↔ DESCENDO
       ↔ FREANDO
```

## 🧪 Simulação e Testes

O projeto inclui testbenches completos para validação de todos os módulos e cenários de operação.

### Como Executar as Simulações
```bash
# Clone o repositório
git clone https://github.com/DevlTz/Logic_Circuits_Elevator.git
cd Logic_Circuits_Elevator

# Torne o script executável
chmod u+x testbench.sh

# Execute o script de simulação
./testbench.sh
```

### Cenários de Teste

- ✅ Chamadas externas simultâneas
- ✅ Chamadas internas de múltiplos elevadores
- ✅ Casos de borda (térreo e cobertura)
- ✅ Operação de portas com timer
- ✅ Escalonamento com múltiplas requisições
- ✅ Reset e inicialização do sistema


## 🚀 Funcionalidades Implementadas

- ✅ **Controle de 3 elevadores independentes**
- ✅ **32 andares (0-31)**
- ✅ **Algoritmo de escalonamento Scan**
- ✅ **Chamadas externas (subir/descer)**
- ✅ **Chamadas internas (painéis)**
- ✅ **Controle automático de portas**
- ✅ **Sensores de andar**
- ✅ **Displays de status (andar, porta, direção)**
- ✅ **Arquitetura hierárquica modular**
- ✅ **Validação completa via testbench**

## 🛠️ Ferramentas Utilizadas

- **VHDL** - Linguagem de descrição de hardware
- **ModelSim/GHDL** - Simulador VHDL
- **GTKWave** - Visualizador de formas de onda (opcional)

## 📚 Conceitos Aplicados

- Máquinas de Estados Finitos (FSM)
- Arquitetura hierárquica
- Sincronização de processos
- Algoritmos de escalonamento
- Controle em tempo real
- Design modular
- Validação por simulação

## 🤝 Contribuindo

Este é um projeto acadêmico. Para sugestões ou melhorias, entre em contato com os colaboradores.

## 📄 Licença

Este projeto foi desenvolvido para fins educacionais como parte da disciplina de Circuitos Digitais (UFRN).

## 👨‍💻 Autores

Desenvolvido por [Luisa Ferreira](https://github.com/luisaferreirass), [Kauã do Vale](https://github.com/DevlTz), [Cícero Paulino](https://github.com/ciceropaulino) e [Ryan Silvestre](https://github.com/RyanSilv257)
