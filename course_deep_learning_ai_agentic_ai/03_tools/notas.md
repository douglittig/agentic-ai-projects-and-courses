# Dominando Tool Use em LLMs: Um Guia Prático
*Baseado nas aulas de Andrew Ng*

Este guia resume os conceitos fundamentais sobre como Grandes Modelos de Linguagem (LLMs) interagem com ferramentas externas para expandir suas capacidades além da geração de texto.

---

## Capítulo 1: O Que é Tool Use?

### O Conceito
[cite_start]Embora humanos usem ferramentas físicas como martelos e chaves inglesas, para um LLM, "ferramentas" são **funções** de código[cite: 4]. [cite_start]O "Tool Use" (uso de ferramentas) é a capacidade de um LLM decidir, autonomamente, quando solicitar a execução de uma função para realizar uma ação ou coletar informações que ele não possui nativamente[cite: 2, 9].

### Por que usar?
Modelos treinados têm conhecimento estático. [cite_start]Se você perguntar "Que horas são?", um modelo padrão não saberá a resposta[cite: 6, 7].
* **Sem Tools:** O modelo alucina ou pede desculpas.
* **Com Tools:** O desenvolvedor fornece uma função `getCurrentTime`. [cite_start]O LLM reconhece que precisa dessa informação e solicita sua execução para dar uma resposta útil[cite: 8, 12].

---

## Capítulo 2: Como Funciona (Mecânica e Sintaxe)

O LLM não executa a ferramenta diretamente; ele apenas **solicita** a execução. [cite_start]O fluxo ocorre em quatro passos[cite: 14, 15, 16]:

1.  **Prompt:** O usuário faz uma pergunta (ex: "Agende uma reunião com Alice").
2.  [cite_start]**Decisão do Modelo:** O LLM analisa as ferramentas disponíveis (ex: `checkCalendar`, `makeAppointment`) e decide qual chamar primeiro[cite: 37].
3.  [cite_start]**Execução (Seu Código):** O sistema detecta o pedido do LLM, executa a função real no backend e captura o resultado[cite: 38].
4.  **Resposta Final:** O resultado é devolvido ao LLM como contexto. [cite_start]O LLM processa essa nova informação e gera a resposta final ou decide chamar outra ferramenta[cite: 39, 40].

### A Evolução da Sintaxe
* [cite_start]**Método Antigo (Prompt Engineering):** Exigia instruções manuais complexas no prompt (ex: "Se precisar de uma ferramenta, imprima FUNCTION: nome_da_função") e parsing manual da resposta[cite: 63, 66].
* **Método Moderno (Nativo):** LLMs atuais são treinados para estruturar essas chamadas nativamente. Bibliotecas como `AISuite` automatizam o processo:
    * [cite_start]Elas leem a **docstring** (comentários) da sua função Python[cite: 181].
    * [cite_start]Convertem isso automaticamente em um **JSON Schema** que explica ao modelo o que a função faz e quais parâmetros ela exige (ex: fuso horário)[cite: 185, 192].

---

## Capítulo 3: Code Execution (Execução de Código)

[cite_start]Uma das ferramentas mais poderosas é permitir que o LLM escreva e execute seu próprio código[cite: 215].

### O Problema da Calculadora
[cite_start]Se você quiser que o LLM faça matemática avançada, criar uma ferramenta separada para cada operação (soma, subtração, raiz quadrada) é ineficiente[cite: 226, 227].

### A Solução
Forneça uma ferramenta de execução de código. [cite_start]O LLM escreve um script Python (ex: para calcular juros compostos ou raiz quadrada), o sistema executa esse script e devolve o resultado numérico[cite: 228, 230]. Isso dá ao modelo flexibilidade quase infinita para resolver problemas lógicos e matemáticos.

### ⚠️ Segurança: O Sandbox
[cite_start]Executar código gerado por IA traz riscos (ex: um modelo pode acidentalmente decidir deletar arquivos do seu projeto)[cite: 249].
* [cite_start]**Prática Recomendada:** Sempre execute esse código em um ambiente isolado (**Sandbox**), como Docker ou E2B, para evitar danos ao sistema principal[cite: 255, 260].

---

## Capítulo 4: MCP - Model Context Protocol

À medida que o ecossistema cresce, conectar LLMs a diferentes fontes de dados tornou-se complexo. [cite_start]O **Model Context Protocol (MCP)** é um padrão aberto para resolver isso[cite: 268, 273].

### O Problema "M x N"
Antes do MCP, se você tivesse 3 apps (M) querendo conectar com Slack, Google Drive e GitHub (N), cada desenvolvedor tinha que escrever conectores manuais para tudo. [cite_start]O trabalho total era a multiplicação das partes ($M \times N$)[cite: 279, 286].

### A Solução MCP ($M + N$)
[cite_start]O MCP padroniza essa conexão, reduzindo o trabalho para uma soma ($M + N$)[cite: 288].
* [cite_start]**MCP Servers:** Criam a ponte com os dados (ex: um servidor que sabe ler o GitHub)[cite: 301].
* [cite_start]**MCP Clients:** Aplicações (como seu chatbot) que consomem esses recursos sem precisar saber os detalhes da API do GitHub[cite: 293, 303].

**Exemplo Prático:**
Um cliente MCP pode pedir "Resuma o arquivo readme.md deste repositório". [cite_start]O servidor MCP do GitHub busca o arquivo e lista os Pull Requests recentes, entregando o contexto pronto para o LLM processar[cite: 302, 307].

---