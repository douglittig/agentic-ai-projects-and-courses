# DeepLearning.AI Agentic AI Course Notes & Labs

Repositório de estudos e práticas do curso de **Agentic AI** da DeepLearning.AI.

## Estrutura do Projeto

Este repositório está organizado em módulos conforme o andamento do curso. Cada módulo contém notas de estudo (`notas.md`) e laboratórios práticos (Jupyter Notebooks) traduzidos e adaptados.

### [01 - Introdução aos Fluxos de Trabalho Agênticos](01_intro_agentic_workflows/notas.md)
*   **Notas**: Conceitos fundamentais de IA Agêntica e a distinção entre LLMs (passivos) e Agentes (ativos).

### [02 - Padrão de Design de Reflexão (Reflection)](02_reflection_pattern/notas.md)
*   **Notas**: O padrão de auto-correção e melhoria iterativa.
*   **Labs**:
    *   [Lab 01: Basic Reflection Agent](02_reflection_pattern/lab_01/lab_01_reflection.ipynb) - Gerador de código Python com loop de reflexão.
    *   [Lab 02: Advanced Reflection](02_reflection_pattern/lab_02/lab_02_reflection.ipynb) - Agente SQL que refina suas queries com base em erros.

### [03 - Ferramentas (Tool Use)](03_tools/notas.md)
*   **Notas**: Como capacitar agentes com ferramentas externas (Function Calling).
*   **Labs**:
    *   [Lab 01: Function Calling Basics](03_tools/lab_01/M3_UGL_1.ipynb) - Introdução ao uso de ferramentas com `aisuite`.
    *   [Lab 02: Email Agent](03_tools/lab_02/M3_UGL_2.ipynb) - Agente capaz de gerenciar emails e realizar ações reais.

### [04 - Dicas Práticas para Construção de IA Agêntica](04_practical_tips_for_building_agentic_AI/notas.md)
*   **Notas**: Melhores práticas, orquestração e avaliação de agentes.
*   **Labs**:
    *   [Lab 01: Evaluator & Optimizer](04_practical_tips_for_building_agentic_AI/lab_01/M4_UGL_1.ipynb) - Avaliação da qualidade de respostas de busca usando métricas personalizadas.

### [05 - Padrões para Agentes Altamente Autônomos](05_patterns_for_highly_autonomous_agents/notas.md)
*   **Notas**: Planejamento, raciocínio avançado e arquiteturas multi-agente.
*   **Labs**:
    *   [Lab 01: Planning with Code](05_patterns_for_highly_autonomous_agents/lab_01/M5_UGL_1_R.ipynb) - "Code as Policy": O agente planeja escrevendo e executando código Python.
    *   [Lab 02: Multi-Agent Creative Team](05_patterns_for_highly_autonomous_agents/lab_02/M5_UGL_2.ipynb) - Orquestração de uma equipe de marketing (Pesquisador, Designer, Redator) para criar uma campanha completa.

---

## 🛠️ Utils (Pacote Utilitário)
A pasta `utils/` na raiz do projeto atua como um pacote Python centralizado contendo funções compartilhadas por todos os laboratórios:
*   **`tools.py`**: Ferramentas de busca (Tavily), catálogo e banco de dados.
*   **`utils.py`**: Funções de visualização (HTML/CSS para notebooks), logs coloridos e helpers gerais.
*   **`inv_utils.py`, `inventory_utils.py`**: Utilitários para simulação de inventário e bancos de dados (TinyDB).

Todos os notebooks foram configurados para importar deste pacote central.

## 🚀 Como usar

1.  **Instale as dependências**:
    ```bash
    pip install -r requirements.txt
    ```
2.  **Configure o ambiente**:
    *   Crie um arquivo `.env` na raiz (use `.env.example` como base).
    *   Adicione suas chaves de API (OpenAI, Tavily, etc).
3.  **Execute os Laboratórios**:
    *   Abra os arquivos `.ipynb` no VS Code ou Jupyter Lab.
    *   Certifique-se de que o kernel Python selecionado é o mesmo onde as dependências foram instaladas.

## 📝 Notas sobre as Traduções
Os notebooks foram traduzidos para **Português do Brasil (PT-BR)** para facilitar o estudo, mantendo os termos técnicos originais em inglês quando apropriado para clareza.
