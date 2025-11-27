# DeepLearning.AI Agentic AI Course Notes & Labs

Repositório de estudos do curso de Agentic AI da DeepLearning.AI.

## Estrutura do Projeto

### [01 - Introdução aos Fluxos de Trabalho Agênticos](01_intro_agentic_workflows/notas.md)
*   Conceitos básicos de IA Agêntica.
*   Diferença entre LLMs e Agentes.

### [02 - Padrão de Design de Reflexão (Reflection)](02_reflection_pattern/notas.md)
*   **Notas**: Resumo sobre o padrão de reflexão.
*   **Labs**:
    *   [Lab 01: Basic Reflection Agent](02_reflection_pattern/lab_01_reflection.ipynb) - Implementação básica de um agente que reflete sobre seu output.
    *   [Lab 02: Advanced Reflection](02_reflection_pattern/lab_02_reflection.ipynb) - Casos de uso mais complexos.

## Como usar

1.  Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```
2.  Configure suas chaves de API (ex: OpenAI) em um arquivo `.env` (use `.env.example` como base se houver).
3.  Execute os notebooks com Jupyter Lab ou VS Code.

## Notas
*   Certifique-se de ter o arquivo `utils.py` correto em cada módulo para executar os labs.
