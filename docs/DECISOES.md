# 📝 Decisões Técnicas e Arquiteturais

Este documento apresenta as principais decisões técnicas adotadas no desenvolvimento da solução, incluindo escolhas de processamento, integração com inteligência artificial e limitações identificadas.

## 1. Tratamento e Preparação dos Dados

**Decisão:** Utilizar Pandas para limpeza, transformação e preparação das transações.

Principais decisões:

- **Datas nulas:** remoção de registros sem data, pois a identificação de movimentações no mesmo dia depende de uma referência temporal válida. Esses registros devem ser contabilizados como dados descartados para não perder rastreabilidade.
- **Conversão de moeda:** normalização das transações em USD para BRL utilizando a taxa de câmbio definida nos dados.
- **Padronização:** conversão das datas para o formato datetime, permitindo agrupamentos e ordenações temporais.

**Justificativa:** Manter os dados em formatos consistentes antes da aplicação das regras de negócio.

## 2. Processamento Determinístico vs. IA Generativa

**Decisão:** Manter os cálculos e a identificação das regras no Python/Pandas, utilizando a IA para apoiar a interpretação dos resultados e a elaboração dos pareceres.

**Justificativa:** Operações matemáticas e critérios de sinalização precisam ser reproduzíveis e verificáveis. A IA não deve ser responsável por calcular valores ou determinar sozinha a aplicação das regras.

Essa separação também facilita a validação dos resultados.

## 3. Regras de Detecção

Foram implementadas duas regras principais:

- **Possível fracionamento (Smurfing):** identificação de três ou mais movimentações no mesmo dia, com soma superior a R$ 50.000,00 e valores individuais inferiores a R$ 20.000,00.
- **Valor atípico:** identificação de transações superiores a cinco vezes a mediana histórica do cliente, considerando pelo menos quatro operações.

Os limites são parâmetros definidos para a lógica de análise deste projeto e não devem ser interpretados isoladamente como limites legais de comunicação.

## 4. Recuperação de Referências Regulatórias (RAG)

**Decisão:** Utilizar uma base de conhecimento estruturada em Python para fornecer referências regulatórias relacionadas às regras identificadas.

O fluxo funciona da seguinte forma:

1. O processamento determinístico identifica a ocorrência.
2. O sistema recupera a referência correspondente na base de conhecimento.
3. O contexto regulatório é inserido no prompt enviado ao Gemini.
4. O modelo utiliza os dados processados e a referência fornecida para elaborar o parecer.

As referências consideradas incluem a Circular BACEN nº 3.978/2020 e a Carta Circular nº 4.001/2020.

**Justificativa:** Fornecer contexto regulatório específico durante a geração do parecer, reduzindo a dependência de conhecimento exclusivamente gerado pelo modelo.

**Limitação:** A recuperação atual utiliza associação baseada nas regras identificadas e uma base estática em memória. Não há busca vetorial ou indexação semântica de documentos.

## 5. Function Calling e Investigação Contextual

**Decisão:** Utilizar Function Calling para permitir que o agente consulte informações específicas dos clientes durante a investigação.

Foram disponibilizadas ferramentas para consultar:

- Histórico de transações.
- Operações realizadas em uma data específica.
- Distribuição de movimentações por canal.

**Justificativa:** Evitar o envio indiscriminado de todas as informações ao modelo e permitir consultas direcionadas conforme a necessidade da análise.

**Trade-off:** A abordagem exige controle das funções disponíveis e tratamento adequado das respostas e falhas de execução.

## 6. Processamento Concorrente

**Decisão:** Utilizar `ThreadPoolExecutor` para executar investigações de diferentes clientes em paralelo.

O processamento foi configurado com até cinco workers simultâneos.

**Justificativa:** Como as investigações dependem de chamadas externas à API, o processamento concorrente permite aproveitar os períodos de espera e reduzir o tempo total de execução.

**Trade-off:** A concorrência exige atenção ao controle de erros, limites de requisição e acesso compartilhado aos recursos.

## 7. Conciliação das Análises

**Decisão:** Comparar as sinalizações determinísticas com as classificações e justificativas produzidas pelo agente.

A etapa consolida informações como:

- Volume financeiro.
- Regras acionadas.
- Nível de risco atribuído pelo agente.
- Tipologia suspeita.
- Justificativa da análise.

Os resultados são exportados em CSV e JSON.

**Justificativa:** Facilitar a identificação de concordâncias e divergências entre as abordagens.

**Limitação:** A comparação não representa uma medição de precisão ou redução de falsos positivos, pois não existe uma base de referência previamente validada.

## 8. Limitações e Possibilidades de Evolução

A implementação atual utiliza arquivos locais e mantém os dados em memória.

Como possibilidades de evolução, foram identificadas:

- Persistência dos dados em banco de dados.
- Recuperação semântica de documentos regulatórios.
- Tratamento de erros e novas tentativas de chamadas à API.
- Controle de limites de requisição.
- Testes automatizados das regras de negócio.
- Implementação de uma arquitetura multiagente com agentes especializados em triagem, investigação e elaboração de pareceres.

A arquitetura multiagente é uma proposta de evolução, não uma funcionalidade implementada na versão atual.

## Considerações Finais

As decisões arquiteturais buscaram equilibrar confiabilidade dos cálculos, flexibilidade na investigação e eficiência no processamento.

A separação entre regras determinísticas e IA generativa permite combinar resultados reproduzíveis com análise contextual, mantendo a necessidade de validação humana nas decisões relacionadas a compliance.
