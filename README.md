# 🛡️ Análise Inteligente de Transações Financeiras | PLD e Antifraude

Projeto desenvolvido em Python para automatizar a análise de transações financeiras e apoiar a identificação de possíveis indícios de lavagem de dinheiro e comportamentos atípicos.

A solução combina processamento determinístico de dados com inteligência artificial generativa, utilizando Pandas para aplicação de regras de negócio e agentes baseados em Google Gemini para investigação e geração de pareceres.

O objetivo é explorar como diferentes abordagens podem trabalhar em conjunto para tornar as análises financeiras mais estruturadas, rastreáveis e eficientes.

## 🎯 Contexto e Objetivo

Em ambientes financeiros, a identificação de operações suspeitas exige mais do que a análise isolada de transações. É necessário considerar padrões de movimentação, histórico dos clientes, comportamento financeiro e critérios regulatórios.

Nesse contexto, o projeto foi estruturado para combinar duas abordagens complementares:

- **Análise determinística:** aplicação de regras de negócio e cálculos estatísticos para identificar movimentações atípicas.
- **Análise com IA generativa:** investigação contextual das ocorrências, considerando informações adicionais do cliente e referências regulatórias.

## 🏗️ Arquitetura da Solução

### 1. Processamento de Dados e Fundamentação Regulatória

A primeira etapa concentra o processamento estruturado das transações utilizando Pandas.

Principais operações:

- Limpeza e preparação dos dados.
- Conversão cambial.
- Identificação de possíveis operações de fracionamento (*smurfing*).
- Detecção de movimentações atípicas considerando o histórico e a mediana do cliente.
- Fundamentação dos pareceres com referências regulatórias por meio de uma abordagem de RAG.

As referências utilizadas incluem a Circular BACEN nº 3.978/2020 e a Carta Circular nº 4.001/2020.

### 2. Investigação com Agentes de IA e Function Calling

Na segunda etapa, foi desenvolvido um agente utilizando Google Gemini e Function Calling para investigar os clientes sinalizados.

O agente utiliza ferramentas específicas para consultar informações como:

- Histórico de transações.
- Canais de movimentação mais utilizados.
- Perfil de risco do cliente.

Para otimizar o processamento de múltiplas análises, foi utilizado `ThreadPoolExecutor`, permitindo a execução concorrente das investigações.

Em um lote de 10 clientes, o processamento apresentou tempo de execução aproximado de 17 a 19 segundos no ambiente utilizado.

### 3. Conciliação e Validação das Análises

Após as investigações, os resultados são consolidados por meio de um processo de confronto entre as sinalizações das regras determinísticas e os pareceres gerados pelo agente.

Essa etapa permite comparar as duas abordagens e organizar os resultados em arquivos estruturados nos formatos CSV e JSON.

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Aplicação |
|---|---|
| Python 3.10+ | Desenvolvimento da solução |
| Pandas | Tratamento e análise de dados |
| Google Gemini | Análise contextual com IA generativa |
| Function Calling | Consulta estruturada de informações |
| ThreadPoolExecutor | Processamento concorrente |
| RAG | Fundamentação com referências regulatórias |
| JSON e CSV | Estruturação e exportação dos resultados |
| python-dotenv | Gerenciamento de variáveis de ambiente |

## 📂 Estrutura do Repositório

```text
pld-antifraude-com-ia/
│
├── .env.example
├── requirements.txt
│
├── dados/
│
├── docs/
│   ├── DECISOES.md
│   └── USO_DE_IA.md
│
├── nivel_1/
│   └── nivel_1.ipynb
│
├── nivel_2/
│   ├── agente.py
│   ├── tools.py
│   └── confronto.py
│
└── outputs/
```

## 🚀 Como Executar o Projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/SouzaaCarol/pld-antifraude-com-ia.git
cd pld-antifraude-com-ia
```

### 2. Criar e ativar o ambiente virtual

```bash
python -m venv venv
```

No Windows:

```bash
.\venv\Scripts\Activate.ps1
```

No Linux ou macOS:

```bash
source venv/bin/activate
```

### 3. Instalar as dependências

```bash
pip install -r requirements.txt
```

### 4. Configurar a chave da API

Crie um arquivo `.env` na raiz do projeto, utilizando o `.env.example` como referência.

Adicione sua chave de API:

```env
GEMINI_API_KEY=sua_chave_aqui
```

### 5. Executar a solução

**Processamento determinístico e RAG:**

Abra e execute o notebook `nivel_1/nivel_1.ipynb`.

**Investigação com agente de IA:**

```bash
python nivel_2/agente.py
```

**Conciliação dos resultados:**

```bash
python nivel_2/confronto.py
```

## 🔎 Considerações sobre o Projeto

A solução explora a integração entre regras determinísticas e inteligência artificial generativa em um cenário de prevenção à lavagem de dinheiro.

As análises produzidas pela IA devem ser consideradas apoio à investigação, não substituindo a validação humana ou os processos formais de compliance.

A implementação atual contempla o processamento determinístico, a investigação com agente de IA e a conciliação dos resultados.

Como possibilidade de evolução, está prevista a exploração de uma arquitetura multiagente, com agentes especializados em diferentes etapas da investigação e elaboração dos pareceres.

## 👩‍💻 Autora

**Ana Carolina Martins Souza**

Projeto desenvolvido com foco em Python, análise de dados, automação de processos e aplicações de inteligência artificial no contexto financeiro.
