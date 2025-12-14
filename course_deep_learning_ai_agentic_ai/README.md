# DeepLearning.AI Agentic AI Course Notes & Labs

Repositório de estudos do curso de Agentic AI da DeepLearning.AI.

## Estrutura do Projeto

### [01 - Introdução aos Fluxos de Trabalho Agênticos](01_intro_agentic_workflows/notas.md)
*   Conceitos básicos de IA Agêntica.
*   Diferença entre LLMs e Agentes.

### [02 - Padrão de Design de Reflexão (Reflection)](02_reflection_pattern/notas.md)
*   **Notas**: Resumo sobre o padrão de reflexão.
*   **Labs**:
    *   [Lab 01: Basic Reflection Agent](02_reflection_pattern/lab_01/lab_01_reflection.ipynb) - Geração de gráficos com reflexão. (Traduzido para PT-BR)
    *   [Lab 02: Advanced Reflection](02_reflection_pattern/lab_02/lab_02_reflection.ipynb) - Geração de SQL com reflexão. (Traduzido para PT-BR)
    *   **Utils**: Um arquivo `utils.py` unificado na pasta `02_reflection_pattern/` serve a ambos os laboratórios.

## Como usar

1.  Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```
2.  Configure suas chaves de API (ex: OpenAI, Anthropic) em um arquivo `.env`.
3.  Execute os notebooks com Jupyter Lab ou VS Code.

## Notas Importantes
*   **Unificação**: As funções auxiliares de ambos os laboratórios foram consolidadas em `02_reflection_pattern/utils.py`. Os notebooks foram configurados para importar deste local.
