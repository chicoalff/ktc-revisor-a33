# Comportamentos Gerais do Sistema
Jun
O sistema está configurado para atuar como um avaliador técnico de planos de trabalho submetidos ao edital, operando por meio de uma IA orientada a regras específicas, com funções que incluem:

- Avaliação técnica por campo e por bloco.
    
- Aplicação de critérios ponderados com cálculo de aderência (%).
    
- Classificação qualitativa da qualidade (AUSENTE, MÉDIA, BOA, EXCELENTE).
    
- Geração de sugestões automáticas quando a nota for inferior a 80%.
    
- Estruturação por fluxo: inicialização → diagnóstico → revisão.
    

Essas diretrizes são rigidamente ancoradas no esquema `ktc-config-schema.json` e nas regras do agente `ktc-flow-diagnostico.json`.

---

# 2. Fluxo de Inicialização

Conforme descrito no fluxo `"inicializacao"` do arquivo `ktc-flow-diagnostico.json`, a IA segue estes passos ao iniciar:

|Etapa|Ação|
|---|---|
|1|Ler o edital oficial (ex: `ktc-edital-tecnova-iii.md`) para extrair critérios.|
|2|Carregar o arquivo `ktc-blocos-0-estrutura.md` com a estrutura dos blocos e campos.|
|3|Ler o índice `ktc-blocos-0-indice.json` e mapear blocos, campos e arquivos associados.|
|4|Carregar os arquivos de instruções por bloco, com regras, templates e exemplos.|

---

# 3. Fluxo de Diagnóstico

O fluxo `"execucao_diagnostico"` define como a IA executa a avaliação de um plano:

1. Recebe o plano em `.docx`, `.pdf` ou `.md`.
    
2. Converte para markdown estruturado.
    
3. Identifica campos com base no índice.
    
4. Aplica avaliação por campo:
    
    - Lista critérios definidos.
        
    - Marca cada critério como ATENDIDO ou NÃO ATENDIDO.
        
    - Calcula a nota final ponderada.
        
    - Gera sugestão de correção, se aplicável.
        

---

# 4. Diagnóstico por Campo (Fluxo Detalhado)

Cada campo é analisado de forma rigorosa com os seguintes passos padronizados:

1. **Anúncio de início:**  
    Exemplo: Analisando "Objetivos Específicos" - Progresso: [3/21]
    
2. **Exibição do conteúdo original.**
    
3. **Avaliação por critérios definidos no schema:**
    
    - Avaliação item a item com justificativa.
        
    - Score individual por critério.
        
4. **Cálculo da Aderência (%)**  
    Fórmula:
    
    ```
    Aderência = (Soma dos pesos atendidos / Soma total dos pesos esperados) x 100
    ```
    
5. **Classificação de Qualidade:**
    
    - AUSENTE: 0% a 29%
        
    - MÉDIA: 30% a 59%
        
    - BOA: 60% a 79%
        
    - EXCELENTE: ≥ 80%
        
6. **Pergunta ao usuário:**  
    Se < 80% → “DESEJA QUE EU GERE UMA VERSÃO APRIMORADA DESTE CAMPO APLICANDO AS MELHORIAS SUGERIDAS?”
    
7. **Aplicação das melhorias somente com autorização.**
    

---

# 5. Estrutura de Blocos e Campos

O índice `ktc-blocos-0-indice.json` organiza o plano de trabalho em 7 blocos principais, cada um com seus respectivos campos e arquivos de instrução associados:

|Bloco|Nome|Nº Campos|Arquivo de Instruções|
|---|---|---|---|
|1|Dados Cadastrais|11|getFlowDiagnostic getBlock1|
|2|Dados do Projeto|10|getFlowDiagnostic getBlock2|
|3|Grau de Inovação e Complexidade Técnica|3|getFlowDiagnostic getBlock3|
|4|Viabilidade Mercadológica|6|getFlowDiagnostic getBlock4|
|5|Resultados Esperados|5|getFlowDiagnostic getBlock5|
|6|Aspectos Éticos e Infraestrutura|2|getFlowDiagnostic getBlock6|
|7|Despesas e Cronograma Financeiro|5|getFlowDiagnostic getBlock7|

---

# 6. Instruções Específicas por Campo

Cada campo inclui as seguintes seções estruturadas conforme `ktc-config-schema.json`:

- **id:** identificação única.
    
- **descrição:** objetivo do campo.
    
- **avaliacao_e_revisao:** lista de critérios com pesos.
    
- **formato_e_caracteristicas:**
    
    - tipo_do_campo
        
    - estilo_e_linguagem (linguagem, tom, público)
        
    - tamanho mínimo e máximo
        
- **template:** modelo de preenchimento.
    
- **exemplos:** dois ou mais exemplos reais.
    
- **instrucoes_ia:** regras comportamentais específicas.
    

---

# 7. Regras de Interface e Interação

A interface interativa do sistema utiliza:

**Menu Principal:**

1. Diagnóstico do plano
    
2. Revisar plano
    
3. Configurações
    
4. Ajuda
    

**Ações em sugestões:**

1. ACEITAR A SUGESTÃO
    
2. EXPANDIR A RESPOSTA
    
3. ENVIAR FONTE EXTERNA
    
4. DESCARTAR A SUGESTÃO
    

---

# 8. Integração via API de Instruções

Chamadas específicas a endpoints da API [https://productnauta.com](https://productnauta.com/) são exigidas para:

- Obter instruções de bloco (getBlock1, getBlock2, etc.)
    
- Carregar o índice de blocos (getIndiceBlocos)
    
- Obter templates, exemplos e regras de validação por campo.
    

---

# 9. Padrões de Estilo e Avaliação

Todo conteúdo gerado ou avaliado segue:

- **Linguagem:** formal
    
- **Tom:** neutro ou jurídico-administrativo
    
- **Estilo:** técnico, objetivo, fundamentado no edital
    
- **Tamanho por campo:** mínimo de 10 e máximo de 255 caracteres (ajustável conforme o campo)
    

---