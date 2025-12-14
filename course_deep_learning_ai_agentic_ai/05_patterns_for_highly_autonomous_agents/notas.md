# Guia de Estudos: Padrões de Design para Agentes Autônomos
*Baseado no Módulo 5 do curso de Andrew Ng*

Este módulo final explora como evoluir de sequências de passos rígidas ("hard-coded") para sistemas onde o próprio Grande Modelo de Linguagem (LLM) decide quais passos tomar (Planning) e como múltiplos agentes podem colaborar para resolver tarefas complexas (Multi-Agent Systems).

---

## 1. O Padrão de Planejamento (Planning Workflow)

### O Conceito
Em sistemas tradicionais, o desenvolvedor define a ordem exata das ações (ex: primeiro faça A, depois B). No padrão de planejamento, permitimos que o LLM decida a sequência de ferramentas a serem usadas para resolver uma tarefa, garantindo flexibilidade para lidar com solicitações imprevistas.

### Exemplo Prático: Loja de Óculos de Sol
Imagine um cliente perguntando: *"Você tem óculos de sol redondos em estoque que custem menos de $100?"*.
Para responder, o agente precisa executar uma lógica complexa:
1.  Verificar as descrições dos produtos para filtrar o formato "redondo".
2.  Checar o estoque desses itens específicos.
3.  Verificar o preço dos itens que estão em estoque.

### O Fluxo de Execução
Ao invés de programar essa ordem manualmente, você fornece ao LLM um conjunto de ferramentas (como `getItemDescription`, `checkInventory`, `getItemPrice`) e pede que ele gere um plano.



O processo ocorre em um loop:
1.  **Geração do Plano:** O LLM analisa a pergunta e retorna um plano passo a passo.
2.  **Execução Sequencial:**
    * O sistema pega o primeiro passo e o envia para um LLM executar a ferramenta correspondente.
    * O resultado (output) desse passo é usado como contexto para executar o próximo passo.
    * Isso continua até o plano ser concluído e a resposta final gerada para o usuário.

---

## 2. Criando e Formatando Planos

Para que o sistema execute o plano de forma confiável via código, a saída do LLM deve ser estruturada.

### Formatos de Saída
* **JSON:** É o formato mais recomendado. O prompt do sistema deve instruir o LLM a criar um plano em formato JSON. Isso facilita para o seu código fazer a leitura (parsing) de qual ferramenta chamar e quais argumentos usar.
* **Outros formatos:** XML e Markdown também são usados. Texto simples (plain text) deve ser evitado por ser difícil de processar programaticamente.



---

## 3. Planejamento com Execução de Código (Code as Action)

Uma variação poderosa do planejamento é permitir que o LLM escreva e execute **código** (geralmente Python) como parte do plano, em vez de apenas chamar funções pré-definidas.

### O Problema da "Explosão de Ferramentas"
Imagine que você tem uma planilha de vendas de café. Se o usuário perguntar *"Qual mês teve a maior venda de chocolate quente?"* ou *"Quantas transações únicas ocorreram?"*, você precisaria criar ferramentas específicas para cada operação possível (`get_max`, `get_unique`, `filter_rows`, etc.). Isso torna o sistema ineficiente e difícil de manter.

### A Solução via Código
Ao instruir o LLM a *"escrever código Python para resolver a consulta"*:
* O LLM pode usar bibliotecas poderosas de análise de dados (como `pandas`) para carregar o arquivo e manipular os dados.
* O "plano" se torna o próprio script gerado: carregar dados -> filtrar data -> ordenar -> selecionar.
* **Vantagem:** O LLM aproveita o conhecimento que já possui sobre milhares de funções de bibliotecas existentes, eliminando a necessidade de o desenvolvedor criar ferramentas personalizadas para cada tipo de pergunta.

**Nota de Segurança:** A execução de código gerado por IA deve ser feita sempre em um ambiente isolado (**sandbox**) para evitar riscos de segurança, como exclusão de arquivos ou acesso indevido.

---

## 4. Workflows Multi-Agentes

Em vez de tentar criar um único agente "faz-tudo", um sistema multi-agente utiliza uma coleção de agentes especializados colaborando entre si.

### A Analogia da Equipe
Assim como você não contrataria uma única pessoa para fazer todas as funções em uma empresa, você pode decompor uma tarefa complexa em papéis distintos.

### Exemplo: Equipe de Marketing
Para criar uma brochura de marketing para óculos de sol, você pode ter três agentes especializados:
1.  **Pesquisador:** Analisa tendências de mercado e competidores. (Ferramenta: Busca Web).
2.  **Designer Gráfico:** Cria visualizações e arte. (Ferramentas: Geração de imagem ou código para gráficos).
3.  **Redator:** Transforma a pesquisa e as imagens em texto de marketing. (Ferramenta: Geração de texto).

Para criar esses agentes, você utiliza prompts de sistema diferentes, definindo a "persona" e as ferramentas específicas de cada um.

---

## 5. Padrões de Comunicação

Como esses agentes conversam entre si? Existem diferentes arquiteturas para organizar essa colaboração:

### 1. Padrão Linear
Os agentes atuam em uma sequência fixa e pré-determinada.
* *Fluxo:* Pesquisador faz o relatório -> Passa para o Designer -> Passa para o Redator -> Saída Final.
* Ideal para tarefas com etapas claras e dependências sequenciais.

### 2. Padrão Hierárquico (Gerente/Manager)
Um agente atua como "Gerente" e coordena os outros agentes "trabalhadores".
* O LLM "Gerente de Marketing" recebe a tarefa, decide delegar uma parte ao Pesquisador, recebe a resposta, depois delega ao Designer, e assim por diante.
* Isso é similar ao padrão de planejamento, onde o gerente é o "cérebro" que orquestra as chamadas aos "sub-agentes".



### 3. Padrões Complexos (All-to-All)
Qualquer agente pode falar com qualquer outro a qualquer momento.
* Todos os agentes são avisados da existência dos outros e conversam livremente até que a tarefa seja considerada concluída.
* É um padrão mais caótico e difícil de prever, mas pode ser útil para tarefas de brainstorming ou processos criativos onde se tolera alguma imprevisibilidade em troca de inovação.