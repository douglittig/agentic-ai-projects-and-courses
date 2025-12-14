# Capítulo 2: O Padrão de Design de Reflexão (Reflection)

*Baseado no Módulo 2 do curso de Andrew Ng (DeepLearning.AI)*

---

## Introdução: A Arte da Autocrítica

O padrão de design de **Reflexão** é surpreendentemente fácil de implementar e oferece ganhos tangíveis de desempenho. A premissa é simples: assim como humanos revisam um rascunho de e-mail antes de enviá-lo para corrigir erros ou melhorar o tom, Grandes Modelos de Linguagem (LLMs) podem ser solicitados a revisar seus próprios outputs.

O fluxo básico consiste em não aceitar a primeira resposta (V1) como definitiva. Em vez disso, passamos o output V1 de volta para o mesmo modelo (ou um diferente) com instruções para criticar, buscar erros e gerar uma versão melhorada (V2).

> **Nota Crítica:** A reflexão não é mágica. Ela não garante 100% de precisão, mas frequentemente oferece um aumento modesto e valioso na performance, especialmente em tarefas onde a "geração direta" (zero-shot) tende a falhar.

---

## 2.1. Geração Direta vs. Reflexão

Na **geração direta** (ou *zero-shot prompting*), solicitamos ao LLM que realize uma tarefa de uma só vez. Embora rápido, esse método é propenso a erros em tarefas complexas. Estudos mostram que a reflexão supera a geração direta em uma variedade de tarefas, desde a criação de código até a escrita de ensaios.

### Quando usar a Reflexão?
A reflexão brilha em cenários específicos onde a validação é crucial:
* **Dados Estruturados:** Ao gerar JSON ou HTML complexo, a reflexão pode identificar erros de sintaxe ou aninhamento incorreto.
* **Tarefas Criativas com Restrições:** Exemplo: Gerar nomes de domínio. O modelo pode criar nomes e, em seguida, refletir sobre eles para eliminar opções difíceis de pronunciar ou com conotações negativas não intencionais.
* **Sequências Longas:** Revisar se instruções passo a passo (como uma receita ou tutorial) não pularam etapas lógicas.

---

## 2.2. O Papel dos Modelos de Raciocínio

Nem todos os LLMs são iguais. Modelos de raciocínio (*reasoning models*), às vezes chamados de "thinking models", tendem a ser superiores na etapa de crítica. Uma estratégia eficaz é usar um modelo rápido para gerar o rascunho (V1) e um modelo de raciocínio mais robusto para encontrar bugs e sugerir melhorias para a V2.

---

## 2.3. Feedback Externo: Quebrando a Alucinação

A forma mais poderosa de reflexão não depende apenas do LLM "pensar" sobre o que escreveu, mas sim de expô-lo a **Feedback Externo**. Se a única fonte de informação for o próprio LLM, ele pode alucinar ou não perceber erros sutis. O feedback externo traz dados da realidade para o processo.

### Exemplos Práticos de Feedback Externo

1.  **Execução de Código:**
    Ao pedir para o LLM escrever código, a melhor reflexão é *executar* o código. Se houver um erro de sintaxe, o log de erro é passado de volta ao LLM. Isso fornece informações concretas para ele corrigir o script.

2.  **Validação de Consultas (SQL):**
    *Caso de Estudo (Lab):* Um agente gera uma query SQL para analisar vendas. O texto da query parece correto sintaticamente. No entanto, ao executar a query no banco de dados, o resultado retorna um valor de vendas negativo (devido à forma como o banco registra saídas de estoque).
    * **Sem Feedback Externo:** O LLM diria que a query está correta.
    * **Com Feedback Externo:** O agente recebe o resultado negativo (`-190571.46`), percebe que vendas não podem ser negativas, e reescreve a query aplicando uma função absoluta (`ABS`) ou multiplicando por -1. A execução revela erros semânticos que a revisão textual ignora.

3.  **Verificação de Fatos e Restrições:**
    Ferramentas simples podem contar palavras (para garantir limites de tamanho) ou fazer buscas na web para verificar datas históricas (ex: data de construção do Taj Mahal), alimentando essas correções de volta ao modelo para a versão final.

---

## 2.4. Reflexão Multimodal (Visão Computacional)

A reflexão não se limita a texto. Em workflows de geração de gráficos, podemos usar a capacidade visual dos modelos atuais.

* **O Problema:** Um código Python gerado pode criar um gráfico tecnicamente funcional, mas visualmente ilegível (ex: um gráfico de barras empilhadas confuso).
* **A Solução Agêntica:** O workflow gera o gráfico (V1), tira um "snapshot" (imagem) dele e passa essa imagem de volta para um LLM multimodal com a instrução: *"Critique este gráfico visualmente"*. O modelo "vê" a confusão e reescreve o código para gerar um gráfico mais limpo e legível.

---

## 2.5. Avaliação (Evals) de Workflows de Reflexão

Adicionar reflexão torna o sistema mais lento e caro. Portanto, você deve provar que o ganho de qualidade justifica o custo.

### Métricas Objetivas vs. Subjetivas

* **Objetivas:** Para tarefas com resposta certa (ex: queries SQL), crie um conjunto de dados de "gabarito" (ground truth) e meça a porcentagem de acerto com e sem reflexão.
* **Subjetivas (LLM como Juiz):** Para avaliar a qualidade de um texto ou gráfico, usar um LLM para comparar "Imagem A vs. Imagem B" é problemático devido ao **viés de posição** (LLMs tendem a preferir a primeira opção apresentada).

**A Melhor Prática para Avaliação Subjetiva:**
Em vez de comparação direta, use **Rubricas**. Peça ao LLM para avaliar um único output com base em critérios binários claros (ex: "O gráfico tem título?", "Os eixos têm rótulos?", "O tom é profissional?"). Somar esses pontos gera uma avaliação mais consistente e calibrada do que pedir uma nota de 1 a 5.

---

## 2.6. Conclusão do Módulo

Se você está gastando muito tempo refinando prompts (prompt engineering) e o desempenho estagnou, pare. Em vez de tentar criar o "prompt perfeito" de uma vez só, implemente um passo de reflexão. E, se possível, integre feedback externo (execução de código, ferramentas de busca). Isso geralmente quebra o platô de desempenho e leva sua aplicação agêntica para o próximo nível.

---
*Fim do Capítulo 2. Baseado no Módulo 2 do curso "Introduction to Agentic Workflows".*