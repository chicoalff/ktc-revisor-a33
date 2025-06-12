metadata:
  versao: "STRING" # Versão do documento. Ex: "1.0"
  arquivo: "STRING" # Nome do arquivo. Ex: “ktc-bloco-1-instrucoes.yml"
  descricao_arquivo: "STRING" # Descrição detalhada do propósito do arquivo.
  autor: "STRING" # Nome do autor. Ex: "Chico Alff"
  data_ultima_edicao: "STRING" # Data da última alteração. Ex: "YYYY-MM-DD HH:MM:SS" (formato datetime)
  nome_do_bloco_detalhado: "STRING" # Nome completo do bloco detalhado no arquivo. Ex: "1 - Dados Cadastrais"

blocos:
  - nome_do_bloco: "STRING" # Nome do bloco. Ex: "1 - Dados Cadastrais"
    campos:
      [nome_do_campo_snake_case]: # Chave do campo em snake_case. Ex: empresa_proponente
        id: "STRING" # Identificador único do campo dentro do bloco. Ex: "1.1"
        descricao: "STRING" # Descrição breve do campo.
        avaliacao_e_revisao: # Critérios para a IA avaliar e revisar o campo.
          criterio_1: "STRING"
          # ... adicionar outros critérios conforme necessário (criterio_2, criterio_3, etc.)
        formato_e_caracteristicas: # Propriedades de formatação e características esperadas.
          tipo_do_campo: "STRING" # Tipo de dado. Ex: "descritivo", "numérico", "texto corrido técnico"
          estilo_e_linguagem:
            linguagem: "STRING" # Linguagem. Ex: "formal", "técnica", "jurídico"
            tom: "STRING" # Tom. Ex: "jurídico-administrativo", "direto", "neutro"
            publico_alvo: "STRING" # Público-alvo. Ex: "especialistas", "gestores"
          estrutura:
            tamanho_campo:
              minimo: "STRING" # Tamanho mínimo. Usar "Não se aplica" quando cabível.
              máximo: "STRING" # Tamanho máximo. Usar "Não se aplica" quando cabível.
            elementos_de_formatação:
              tipo_do_elemento: "STRING" # Tipo de elemento. Ex: "Texto curto", "Número", "Texto corrido"
              detalhes: "STRING" # Detalhes adicionais de formatação.
        instrucoes_ia: # Instruções detalhadas para o modelo de IA.
          instrucao_1: "STRING"
          # ... adicionar outras instruções conforme necessário (instrucao_2, instrucao_3, etc.)
        template: "STRING" # Modelo de como o campo deve ser formatado.
        exemplos: # Lista de exemplos de preenchimento correto.
          - "STRING"
          - "STRING"
          # ... adicionar mais exemplos conforme necessário

      # ... (adicionar mais campos aqui, seguindo a mesma estrutura)

Diretrizes Essenciais:
 * Indentação: Use 2 espaços para cada nível de indentação. Nunca use tabulações.
 * Chaves e Valores:
   * As chaves devem ser descritivas e, preferencialmente, em snake_case (ex: nome_do_bloco, empresa_proponente).
   * Os valores devem ser strings (entre aspas duplas " ou simples ') quando contiverem espaços, caracteres especiais ou forem vazios.
   * Valores booleanos (true/false) ou numéricos podem ser sem aspas, mas prefira strings para consistência (ex: "1.0", "Não se aplica").
 * Listas: Use o hífen - seguido de um espaço para itens de lista (ex: sob blocos, exemplos, avaliacao_e_revisao, instrucoes_ia).
 * Comentários: Utilize # para adicionar comentários explicativos, como visto no modelo.
 * Campos Vazios: Se um campo não tiver um valor aplicável, declare-o com aspas vazias "" ou null se for uma lista ou objeto vazio. Para campos de tamanho onde não se aplica, use a string "Não se aplica".
 * metadata: Todos os campos em metadata são obrigatórios.
 * blocos: O campo blocos deve ser sempre uma lista, mesmo que contenha apenas um bloco.
 * id dos Campos: Os IDs devem seguir o formato "X.Y" onde X é o número do bloco e Y é o número sequencial do campo dentro daquele bloco.
Exemplo de Aplicação (Parte do seu exemplo original):
# ... (seção metadata)

blocos:
  - nome_do_bloco: "1 - Dados Cadastrais"
    campos:
      empresa_proponente:
        id: "1.1"
        descricao: "Nome da empresa proponente conforme CNPJ. Deve corresponder ao CNPJ informado no plano de trabalho."
        avaliacao_e_revisao:
          criterio_1: "Verificar se o CNPJ extraído do documento corresponde ao nome da empresa."
          criterio_2: "Confrontar o CNPJ com o plano de trabalho para consistência."
          # ... outros critérios
        formato_e_caracteristicas:
          tipo_do_campo: "descritivo"
          estilo_e_linguagem:
            linguagem: "formal"
            tom: "jurídico-administrativo"
            publico_alvo: "especialistas, gestores"
          estrutura:
            tamanho_campo:
              minimo: "19"
              máximo: "48"
            elementos_de_formatação:
              tipo_do_elemento: "Texto curto"
              detalhes: "Nome empresarial completo, formal, sem abreviações. Escrita capitalizada. Redação objetiva. Estilo institucional."
        instrucoes_ia:
          instrucao_1: "Ao processar, avaliar ou revisar o campo 'empresa_proponente', busque informações em toda a base de conhecimento disponível para verificar a conformidade com o CNPJ informado."
          # ... outras instruções
        template: "Empresa: [Nome da Empresa]"
        exemplos:
          - "Empresa: ABC SOLUÇÕES TECNOLÓGICAS LTDA"
          - "Empresa: INDÚSTRIA DE SOFTWARE INOVADOR S.A."