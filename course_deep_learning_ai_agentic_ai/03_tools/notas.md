# Dominando Tool Use em LLMs: Um Guia Prático
*Baseado nas aulas de Andrew Ng*

Este guia resume os conceitos fundamentais sobre como Grandes Modelos de Linguagem (LLMs) interagem com ferramentas externas para expandir suas capacidades além da geração de texto.

---

## Capítulo 1: O Que é Tool Use?

### O Conceito
Embora humanos usem ferramentas físicas como martelos e chaves inglesas, para um LLM, "ferramentas" são **funções** de código[cite: 4]. O "Tool Use" (uso de ferramentas) é a capacidade de um LLM decidir, autonomamente, quando solicitar a execução de uma função para realizar uma ação ou coletar informações que ele não possui nativamente.

### Por que usar?
Modelos treinados têm conhecimento estático. Se você perguntar "Que horas são?", um modelo padrão não saberá a resposta[cite: 6, 7].
* **Sem Tools:** O modelo alucina ou pede desculpas.
* **Com Tools:** O desenvolvedor fornece uma função `getCurrentTime`. O LLM reconhece que precisa dessa informação e solicita sua execução para dar uma resposta útil.

---

## Capítulo 2: Como Funciona (Mecânica e Sintaxe)

O LLM não executa a ferramenta diretamente; ele apenas **solicita** a execução. O fluxo ocorre em quatro passos:

1.  **Prompt:** O usuário faz uma pergunta (ex: "Agende uma reunião com Alice").
2.  **Decisão do Modelo:** O LLM analisa as ferramentas disponíveis (ex: `checkCalendar`, `makeAppointment`) e decide qual chamar primeiro.
3.  **Execução (Seu Código):** O sistema detecta o pedido do LLM, executa a função real no backend e captura o resultado.
4.  **Resposta Final:** O resultado é devolvido ao LLM como contexto. O LLM processa essa nova informação e gera a resposta final ou decide chamar outra ferramenta.

### A Evolução da Sintaxe
* **Método Antigo (Prompt Engineering):** Exigia instruções manuais complexas no prompt (ex: "Se precisar de uma ferramenta, imprima FUNCTION: nome_da_função") e parsing manual da resposta.
* **Método Moderno (Nativo):** LLMs atuais são treinados para estruturar essas chamadas nativamente. Bibliotecas como `AISuite` automatizam o processo:
    * Elas leem a **docstring** (comentários) da sua função Python.
    * Convertem isso automaticamente em um **JSON Schema** que explica ao modelo o que a função faz e quais parâmetros ela exige (ex: fuso horário).

---

## Capítulo 3: Code Execution (Execução de Código)

Uma das ferramentas mais poderosas é permitir que o LLM escreva e execute seu próprio código.

### O Problema da Calculadora
Se você quiser que o LLM faça matemática avançada, criar uma ferramenta separada para cada operação (soma, subtração, raiz quadrada) é ineficiente.

### A Solução
Forneça uma ferramenta de execução de código. O LLM escreve um script Python (ex: para calcular juros compostos ou raiz quadrada), o sistema executa esse script e devolve o resultado numérico. Isso dá ao modelo flexibilidade quase infinita para resolver problemas lógicos e matemáticos.

### ⚠️ Segurança: O Sandbox
Executar código gerado por IA traz riscos (ex: um modelo pode acidentalmente decidir deletar arquivos do seu projeto).
* **Prática Recomendada:** Sempre execute esse código em um ambiente isolado (**Sandbox**), como Docker ou E2B, para evitar danos ao sistema principal.

---

## Capítulo 4: MCP - Model Context Protocol

À medida que o ecossistema cresce, conectar LLMs a diferentes fontes de dados tornou-se complexo. O **Model Context Protocol (MCP)** é um padrão aberto para resolver isso.

### O Problema "M x N"
Antes do MCP, se você tivesse 3 apps (M) querendo conectar com Slack, Google Drive e GitHub (N), cada desenvolvedor tinha que escrever conectores manuais para tudo. O trabalho total era a multiplicação das partes ($M \times N$).

### A Solução MCP ($M + N$)
O MCP padroniza essa conexão, reduzindo o trabalho para uma soma ($M + N$).
* **MCP Servers:** Criam a ponte com os dados (ex: um servidor que sabe ler o GitHub).
* **MCP Clients:** Aplicações (como seu chatbot) que consomem esses recursos sem precisar saber os detalhes da API do GitHub.

**Exemplo Prático:**
Um cliente MCP pode pedir "Resuma o arquivo readme.md deste repositório". O servidor MCP do GitHub busca o arquivo e lista os Pull Requests recentes, entregando o contexto pronto para o LLM processar.

---