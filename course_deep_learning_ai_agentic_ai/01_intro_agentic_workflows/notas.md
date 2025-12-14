# Introdução à IA Agêntica e Workflows Agênticos

*Baseado no curso de Andrew Ng (DeepLearning.AI)*

---

## Introdução: Além do Hype

O termo "agêntico" (agentic) tem sido cooptado pelo marketing, gerando um hype excessivo. No entanto, ignorando o ruído, o número de aplicações valiosas construídas com IA Agêntica está crescendo rapidamente.

Hoje, workflows agênticos são utilizados para criar agentes de suporte ao cliente, realizar pesquisas profundas, processar documentos legais complexos e sugerir diagnósticos médicos. A grande diferença entre quem sabe construir esses sistemas e quem é menos eficaz reside na disciplina do processo de desenvolvimento, especificamente focado em avaliações (evals) e análise de erros.

---

## 1.1: O Que é IA Agêntica?

A forma como muitos utilizam Grandes Modelos de Linguagem (LLMs) hoje é através de prompting direto (zero-shot): pedimos para a IA escrever um ensaio do início ao fim, de uma só vez. Isso é comparável a pedir a um humano para escrever um texto sem nunca usar a tecla de apagar (backspace). Nem humanos nem IAs produzem seu melhor trabalho dessa forma linear e restrita.

### O Workflow Agêntico
Em contraste, um workflow agêntico é um processo iterativo. Um exemplo de fluxo para escrita de ensaios seria:
1.  Escrever um esboço.
2.  Decidir se precisa de pesquisa na web.
3.  Realizar a pesquisa e baixar páginas.
4.  Escrever o primeiro rascunho.
5.  Revisar o rascunho e identificar partes que precisam de melhoria.
6.  Revisar e finalizar.

> **Definição:** Um workflow de IA agêntica é um processo onde uma aplicação baseada em LLM executa múltiplos passos para completar uma tarefa.

Embora esse processo iterativo possa levar mais tempo, ele entrega um produto final de qualidade muito superior.

---

## 1.2: Graus de Autonomia

Existe um debate desnecessário na comunidade de IA sobre o que é ou não um "verdadeiro agente". É mais produtivo pensar no termo "agêntico" como um adjetivo que denota um espectro de autonomia, e não uma classificação binária.

### O Espectro de Autonomia
1.  **Baixa Autonomia (Fluxos Determinísticos):** O programador define rigidamente a sequência de passos. Exemplo: O código dita "faça uma busca na web", depois "resuma o texto". O LLM apenas gera o texto, mas o controle do fluxo é hard-coded (codificado rigidamente).
2.  **Semi-Autonomia:** O agente pode tomar algumas decisões, como escolher qual ferramenta usar, mas dentro de um conjunto predefinido.
3.  **Alta Autonomia:** O LLM decide a sequência de passos. Exemplo: Diante de um pedido, o LLM decide se precisa pesquisar na web, consultar notícias ou ler papers acadêmicos, e define quando parar ou iterar.

Aplicações de baixa autonomia são extremamente valiosas e confiáveis para processos de negócios atuais. Agentes de alta autonomia são promissores, mas frequentemente menos controláveis e imprevisíveis.

---

## 1.3: Benefícios da IA Agêntica

Existem três benefícios principais em adotar workflows agênticos:

### 1. Desempenho Superior
Dados do benchmark de codificação *Human Eval* mostram que o GPT-3.5 (modelo antigo) envolto em um workflow agêntico supera o desempenho do GPT-4 (modelo mais novo) usado de forma direta (zero-shot).
* **GPT-3.5 (Zero-shot):** ~40% de acerto.
* **GPT-4 (Zero-shot):** ~67% de acerto.
* **GPT-3.5 (Agêntico):** Supera os resultados diretos do GPT-4.

Isso demonstra que um bom workflow é mais determinante para a qualidade do que apenas a inteligência bruta do modelo.

### 2. Paralelismo
Agentes podem realizar tarefas muito mais rápido que humanos através do processamento paralelo. Ao pesquisar um tema, um humano leria uma página por vez. Um agente pode disparar múltiplas buscas e ler/processar dezenas de páginas simultaneamente antes de sintetizar a resposta.

### 3. Modularidade
Workflows agênticos permitem trocar componentes facilmente. Você pode testar diferentes mecanismos de busca (Google, Bing, Tavily) ou diferentes LLMs para etapas específicas do processo, otimizando custos e resultados.

---

## 1.4: Aplicações Práticas

### Processamento de Faturas (Processo Claro)
Tarefas com processos claros são ideais para workflows agênticos.
* **Input:** PDF da fatura.
* **Ação:** Converter PDF para texto -> LLM extrai campos (Empresa, Valor, Data) -> LLM classifica documento -> Atualizar banco de dados via API.
* **Veredito:** Confiável e fácil de implementar.

### Atendimento ao Cliente (Complexidade Média)
Responder a inquéritos básicos (ex: status do pedido).
* **Fluxo:** Extrair detalhes do pedido do e-mail -> Consultar banco de dados de pedidos -> Rascunhar resposta -> (Opcional) Revisão humana -> Enviar.
* **Desafio:** Se o cliente fizer perguntas fora do script (ex: "tem calça preta?"), o agente precisa de mais autonomia para planejar consultas ao inventário.

### Uso de Computador (Alta Complexidade/Experimental)
Agentes que controlam o navegador web para realizar tarefas complexas, como verificar voos em sites de companhias aéreas.
* **Estado atual:** É uma área de pesquisa de ponta. Agentes ainda falham com frequência se o site for lento ou tiver elementos complexos. Promissor, mas não pronto para aplicações críticas.

---

## 1.5: Decomposição de Tarefas

Uma das habilidades mais importantes é a capacidade de decompor uma tarefa complexa em passos discretos.

### A Lógica da Decomposição
Ao planejar um agente, pergunte-se para cada etapa: *"Um LLM ou uma ferramenta específica consegue fazer isso agora?"*.
* Se a resposta for **sim**, implemente.
* Se a resposta for **não**, quebre essa etapa em sub-etapas menores.

**Exemplo: Agente de Pesquisa**
Tentativa 1 (Simples): Gerar ensaio direto. -> *Resultado: Superficial*
Tentativa 2 (Decomposta): Escrever esboço -> Gerar termos de busca -> Escrever ensaio. -> *Resultado: Melhor, mas talvez desconexo*
Tentativa 3 (Refinada): Escrever esboço -> Gerar termos -> Pesquisar -> Escrever rascunho -> **Revisar rascunho (Reflexão)** -> Revisar final.

**Blocos de Construção:**
Seu kit de ferramentas inclui:
1.  **Modelos:** LLMs (texto), Modelos Multimodais (visão/áudio).
2.  **Ferramentas:** APIs (busca, clima, e-mail), RAG (recuperação de informação), Execução de Código.

---

## 1.6: Avaliação (Evals)

É difícil prever antecipadamente onde um workflow agêntico irá falhar. A melhor prática é construir primeiro e analisar as falhas reais.

### Tipos de Avaliação
1.  **Métricas Objetivas:** Testes binários ou numéricos via código.
    * *Exemplo:* O agente mencionou um concorrente proibido? (Busca por string no texto gerado).
2.  **Métricas Subjetivas (LLM como Juiz):** Usar um LLM para avaliar a qualidade do output de outro.
    * *Exemplo:* Pedir a um LLM para dar uma nota de 1 a 5 na coerência do texto. (Nota: LLMs não são ótimos com escalas numéricas, mas funcionam para triagem inicial).

### Análise de Erros
Analise os outputs intermediários (traces). Muitas vezes o erro não está no texto final, mas em um passo intermediário (ex: a busca na web falhou, ou o resumo do artigo foi ruim).

---

## 1.7: Padrões de Design Agêntico

Existem quatro padrões principais para estruturar esses sistemas:

1.  **Reflexão (Reflection):** Solicitar ao LLM que examine seu próprio trabalho para encontrar erros e corrigi-los antes de finalizar. Isso é simples e melhora muito o resultado.
2.  **Uso de Ferramentas (Tool Use):** Capacitar o LLM a chamar funções externas (código, web search) para resolver problemas que ele não consegue resolver apenas com texto.
3.  **Planejamento (Planning):** Permitir que o LLM decida a sequência de passos autônomamente para atingir um objetivo.
4.  **Colaboração Multi-Agente:** Criar múltiplos agentes especializados (ex: um "pesquisador", um "escritor", um "editor") que colaboram entre si.

---

## Apêndice: Configuração do Ambiente

Para experimentar com workflows agênticos localmente, recomenda-se a seguinte configuração:

**Pré-requisitos:** Python 3.10+
**Bibliotecas Essenciais:**

```text
# Ferramentas de Agente e LLM
aisuite==0.1.11
anthropic
mistralai
openai
tavily-python>=0.7.12

# Framework Web e API
fastapi
python-dotenv
uvicorn
```

Instalação Rápida:

Crie um ambiente virtual: python -m venv venv

Ative o ambiente.

Instale as dependências: pip install -r requirements.txt.

Fim do eBook. Baseado no Módulo 1 do curso "Introduction to Agentic Workflows".