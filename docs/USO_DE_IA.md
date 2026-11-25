# 🤖 Uso de Inteligência Artificial

Este documento apresenta como a inteligência artificial foi utilizada durante o desenvolvimento do projeto e como integra o funcionamento da solução.

## 1. IA como Apoio ao Desenvolvimento

O Google Gemini foi utilizado como ferramenta de apoio em atividades como:

- Consulta e interpretação de documentação técnica.
- Apoio na implementação de funcionalidades em Python.
- Estruturação e revisão de prompts.
- Tratamento de respostas em formato JSON.
- Identificação e correção de erros.
- Pesquisa sobre integração com APIs e Function Calling.

As sugestões geradas foram avaliadas e revisadas durante o desenvolvimento, com validação da lógica implementada.

## 2. IA como Parte da Solução

A inteligência artificial também integra o fluxo de análise financeira.

O Google Gemini é utilizado para:

- Interpretar os resultados produzidos pelas regras determinísticas.
- Consultar informações do cliente por meio de ferramentas disponibilizadas ao agente.
- Identificar possíveis tipologias suspeitas.
- Elaborar justificativas e pareceres estruturados.
- Apoiar a comparação entre os resultados determinísticos e as análises contextuais.

## 3. Engenharia de Prompt

Foram utilizadas instruções estruturadas para orientar o comportamento do modelo, definir o contexto da análise e padronizar o formato das respostas.

A utilização de respostas estruturadas em JSON facilita a integração com os scripts Python e a consolidação dos resultados.

## 4. Recuperação de Contexto Regulatório

A solução utiliza uma abordagem simplificada de RAG, na qual referências regulatórias são recuperadas de uma base de conhecimento estruturada e inseridas no contexto enviado ao modelo.

Essa abordagem busca fornecer referências específicas para apoiar a fundamentação dos pareceres.

## 5. Limites e Supervisão Humana

A inteligência artificial é utilizada como ferramenta de apoio à investigação, não como responsável exclusiva pela determinação de irregularidades.

Os resultados gerados podem apresentar inconsistências ou interpretações inadequadas e precisam ser revisados.

As classificações produzidas não substituem análises especializadas, procedimentos internos de compliance ou decisões formais das instituições financeiras.

A solução não realiza comunicação automática de operações ao COAF.

## 6. Considerações Finais

O uso de IA no projeto busca explorar a integração entre processamento determinístico, investigação contextual e automação de análises.

A separação entre os cálculos realizados em Python e a interpretação realizada pelo modelo contribui para manter maior controle sobre as regras de negócio e a rastreabilidade dos resultados.
