# Guia de Estudos: Avaliação e Análise de Erros em Agentes de IA
*Baseado no Módulo 4 do curso de Andrew Ng*

Este guia foca em como transformar um protótipo inicial em um sistema robusto através de processos disciplinados de avaliação (Evals) e análise de erros. A chave para sistemas agênticos eficientes não é apenas construir, mas saber *onde* consertar quando algo dá errado.

---

## 1. A Mentalidade de Desenvolvimento: "Rápido e Sujo"

Ao desenvolver sistemas agênticos, é difícil prever antecipadamente onde o sistema falhará.
* **A Abordagem Recomendada:** Não perca semanas teorizando. Construa um protótipo inicial ("quick and dirty"), mas de forma segura, para ver como ele se comporta na prática.
* **O Objetivo:** Use esse protótipo para identificar onde ele não está funcionando bem e focar seus esforços. É melhor ter algo rodando para criticar do que planejar no vácuo.

---

## 2. Criando Avaliações (Evals)

Uma vez que o sistema roda, você precisa medir seu desempenho. O processo envolve olhar os outputs, identificar falhas e criar métricas. Abaixo estão três exemplos práticos de como estruturar isso:

### Exemplo 1: Processamento de Faturas (Invoice Processing)
* **Tarefa:** Extrair 4 campos obrigatórios de uma fatura e salvar no banco de dados.
* **Problema Identificado:** Ao analisar 20 faturas manualmente, nota-se que o sistema confunde a "Data da Fatura" (issued date) com a "Data de Vencimento" (due date).
* **Como criar o Eval:**
    1.  Separe um conjunto de teste (10 a 20 faturas).
    2.  Anote manualmente a "verdade aterrada" (**Ground Truth**), ou seja, a data de vencimento correta de cada uma.
    3.  Instrua o LLM a formatar a data sempre igual (ex: AAAA-MM-DD) para facilitar a comparação via código.
    4.  Escreva um script simples para comparar se `data_extraida == data_real`.

### Exemplo 2: Assistente de Marketing (Marketing Copy)
* **Tarefa:** Gerar legendas para Instagram para vender produtos (ex: óculos de sol).
* **Restrição:** A legenda deve ter no máximo 10 palavras.
* **Problema Identificado:** O texto gerado é bom, mas frequentemente excede o limite (ex: gera 17 ou 14 palavras).
* **Como criar o Eval:**
    1.  Crie um set de testes com produtos variados (óculos, máquina de café, camisa azul).
    2.  Rode o sistema e use um código Python para contar as palavras do output.
    3.  A métrica é objetiva: `word_count <= 10`.
    * *Nota:* Diferente das faturas, aqui não há uma "resposta correta única" para o texto, mas há uma regra fixa para todos os exemplos.

### Exemplo 3: Agente de Pesquisa (Research Agent)
* **Tarefa:** Escrever artigos sobre tópicos complexos (ex: Avanços na ciência de Buracos Negros).
* **Problema Identificado:** O agente omite pontos-chave importantes que um especialista humano incluiria (ex: falhou em mencionar o "Event Horizon Telescope").
* **Como criar o Eval (LLM-as-a-judge):**
    1.  Defina 3 a 5 "pontos de discussão padrão-ouro" (Gold Standard Talking Points) para cada tópico de teste.
    2.  Use um **LLM como Juiz**. O prompt seria algo como: *"Determine quantos dos 5 pontos padrão-ouro estão presentes no ensaio fornecido"*.
    3.  O LLM retorna uma pontuação (ex: 3 de 5).
    * *Por que usar LLM?* Porque há muitas formas diferentes de escrever sobre o mesmo tópico, dificultando o uso de correspondência exata de palavras (Regex).

---

## 3. A Matriz de Avaliação (O Grid 2x2)

Podemos classificar os tipos de avaliação em dois eixos principais:

| | **Com Ground Truth (Por Exemplo)** | **Sem Ground Truth (Regra Geral)** |
| :--- | :--- | :--- |
| **Avaliação via Código (Objetiva)** | **Ex: Faturas.** Comparar a data extraída com a data correta anotada especificamente para aquela fatura. | **Ex: Marketing.** Contar palavras (limite de 10) via Python. A regra vale para todos, sem resposta única. |
| **Avaliação via LLM (Subjetiva)** | **Ex: Pesquisa.** Verificar se "pontos chave" específicos daquele tópico foram mencionados no texto. | **Ex: Gráficos.** Usar um LLM com uma rúbrica geral (ex: "o gráfico tem eixos claros?") aplicável a qualquer imagem gerada. |

---

## 4. Análise de Erros e Priorização

Quando o sistema falha, não adivinhe onde está o erro. Use a **Análise de Erros** para descobrir qual componente consertar. Agentes são compostos por vários passos; saber qual passo falhou economiza semanas de trabalho.

### Conceitos: Traces e Spans
* **Trace (Rastro):** É o conjunto de todos os outputs intermediários de cada passo do seu agente em uma execução.
* **Span:** É o output de um passo individual.
* **Ação:** Leia os traces. Olhe o que o agente produziu em cada etapa para entender onde a qualidade caiu.

### Estudo de Caso: Agente de Pesquisa (Buracos Negros)
O artigo final ficou ruim. Onde foi o erro?
1.  **Passo 1 (Termos de Busca):** O LLM gerou termos ruins?
    * *Análise:* Um especialista olha e diz "não, os termos 'Black hole theories Einstein' parecem razoáveis".
2.  **Passo 2 (Resultados da Busca):** O motor de busca retornou lixo?
    * *Análise:* O retorno inclui "Astro Kid News" (notícias para crianças), que não é rigoroso o suficiente para um artigo científico.
3.  **Conclusão:** O problema está na qualidade dos resultados da busca (Step 2), não no LLM que gerou os termos (Step 1).

### Método da Planilha (Spreadsheet Analysis)
Crie uma planilha para contar a frequência dos erros nos exemplos que falharam. Isso elimina o "achismo".

* **Cenário Invoice:** Em 20 faturas com erro:
    * 5% das vezes o erro foi do OCR (PDF-to-text) que leu errado.
    * 95% das vezes o texto estava certo, mas o LLM extraiu a data errada.
    * **Ação:** Focar inteiramente no prompt do LLM, ignorar o OCR.

* **Cenário Email de Cliente:** O sistema busca dados no banco e responde o cliente. Em respostas ruins:
    * 75% das vezes o LLM gerou uma query SQL errada (buscou a tabela errada).
    * **Ação:** Melhorar a geração de SQL.

---

## 5. Avaliação por Nível de Componente

Rodar o sistema inteiro (End-to-End) toda vez é caro, lento e ruidoso. Você pode isolar e avaliar apenas um componente crítico.

* **Exemplo (Busca Web):** Se você quer melhorar a pesquisa do agente de ciência:
    1.  Crie uma lista de URLs "Padrão-Ouro" que *deveriam* aparecer para certas perguntas.
    2.  Métrica: Meça a sobreposição (overlap) entre os resultados da sua busca atual e a lista de ouro (usando métricas como F1 Score).
    3.  **Vantagem:** Permite testar rapidamente diferentes motores de busca (Google vs Bing) ou parâmetros de busca sem precisar gerar o artigo final e avaliá-lo.

---

## 6. Como Resolver os Problemas Identificados

Uma vez identificado o componente problemático via análise de erros, as soluções seguem padrões gerais:

### Para Componentes Não-LLM (Busca, RAG, Detecção)
Muitas vezes, esses componentes possuem "botões" (hiperparâmetros) que você pode girar:
* **Busca Web:** Mude o número de resultados retornados ou o intervalo de datas.
* **RAG:** Ajuste o tamanho do chunk (pedaço de texto) ou o limiar de similaridade.
* **Detecção:** Ajuste a sensibilidade (threshold).
* **Substituição:** Troque o provedor (ex: trocar o motor de busca).

### Para Componentes baseados em LLM
1.  **Melhorar Prompts:** Adicione instruções mais explícitas ou use **Few-Shot Prompting** (dar exemplos de input/output desejado dentro do prompt).
2.  **Trocar o Modelo:**
    * Modelos maiores ("Frontier Models") são muito melhores em seguir instruções complexas.
    * *Exemplo de PII (Dados Sensíveis):* Um modelo pequeno (8B parâmetros) falhou ao redigir dados e seguir o formato JSON. Um modelo maior executou perfeitamente.
3.  **Decomposição:** Se o passo é muito complexo para um único prompt, quebre em passos menores (ex: um passo para gerar o pensamento, outro para formatar a resposta).
4.  **Fine-Tuning:** Treinar o modelo com seus dados. É complexo e caro. Use apenas como último recurso se nada mais funcionar e você precisar daqueles últimos pontos percentuais de performance.

**Dica de Ouro:** Leia prompts de outras pessoas (em pacotes open source ou artigos) para "afiar" sua intuição sobre como instruir modelos.

---

## 7. Otimização de Custo e Latência

Muitos times se preocupam com custo cedo demais. A regra é: **Primeiro faça funcionar (qualidade), depois otimize.**

Quando chegar a hora de otimizar:
1.  **Faça o Benchmark:** Cronometre e calcule o custo de cada passo individualmente.
    * *Exemplo:* Se o LLM demora 7s, a busca 5s, mas a escrita final demora 18s, o maior ganho de tempo está na escrita final.
2.  **Estratégias de Latência:**
    * **Paralelismo:** Execute passos independentes (ex: buscar em duas fontes) ao mesmo tempo.
    * **Modelos Menores:** Use modelos mais rápidos/menos inteligentes para passos simples.
3.  **Estratégias de Custo:**
    * Identifique qual passo gasta mais (tokens ou chamadas de API) e veja se um modelo mais barato resolve aquele passo específico.

---

## Resumo do Processo de Desenvolvimento

O desenvolvimento de agentes não é linear, é um ciclo constante entre **Construir** e **Analisar**:

1.  **Início:** Protótipo rápido e sujo.
2.  **Análise Inicial:** Olhar traces manualmente e saídas finais.
3.  **Maturação:** Criar um pequeno set de Evals (10-20 exemplos) para rastrear progresso.
4.  **Refinamento:** Fazer análise de erros rigorosa (planilhas) para priorizar qual componente melhorar.
5.  **Otimização:** Criar evals específicos por componente e, por fim, otimizar custo e latência.