# Dados

Os arquivos de dados **não ficam no repositório** porque são grandes (cada parte tem ~20 MB).

## Arquivos usados

| Arquivo | Origem | Obrigatório? |
|---|---|---|
| `chamados_completo_com_cep_parte1de4.csv` … `parte4de4.csv` | Extração do grupo a partir de `datario.adm_central_atendimento_1746.chamado` (BigQuery, Data.Rio), tipo "Manejo Arbóreo", com CEP e setor censitário acrescentados | Sim (pelo menos uma parte) |
| `dados_finais_setor_v3.csv` | Tabela do grupo de arborização: setores censitários do Censo 2022 com nome do bairro, população e índice de árvore urbana | Não (só para a seção 4.5) |

## Observação

Cada parte cobre **meses diferentes** (a parte 3 tem 12 meses entre jan/2023 e mai/2026). Os números do notebook e do sumário mudam quando se juntam as quatro partes.

## Fonte original

- Chamados 1746: BigQuery, tabela `datario.adm_central_atendimento_1746.chamado` (dados abertos da Prefeitura do Rio).
- Setores censitários: IBGE, Censo 2022.
