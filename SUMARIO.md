# Sumário da qualidade dos dados

**Base analisada:** `chamados_completo_com_cep_parte3de4.csv`, com 42.455 chamados do tipo "Manejo Arbóreo" abertos em 12 meses entre jan/2023 e mai/2026, em 41 colunas. Os números vêm do notebook [`01_qualidade_dados.ipynb`](notebooks/01_qualidade_dados.ipynb) e mudam quando as quatro partes forem juntadas.

## Em linguagem simples

1. **A base é útil, mas não está pronta para uso direto.** As datas vêm como texto e os códigos (bairro, rua, CEP, setor) vêm como número decimal (float), por exemplo `75.0`. Antes de qualquer conta é preciso converter.
2. **Ela mistura coisas diferentes.** Só 70% são pedidos de poda. O resto é remoção, avaliação de risco, árvore caída e mensagens de ouvidoria (13,5%, incluindo elogios e críticas). Sem filtrar, os números de poda ficam inflados.
3. **O mesmo pedido tem até três nomes** ("Poda de árvore em logradouro", "em vias públicas" e "Árvore - Poda"). Em 2026 só aparece "vias públicas". Sem juntar os nomes, a série no tempo ganha "buracos" falsos.
4. **A localização é boa, mas não é perfeita.** 2,5% dos chamados não têm bairro nem rua. 35% não tinham coordenada original e o ponto foi estimado pelo endereço. O bairro do 1746 bate com o bairro do ponto em 92% dos casos, e o bairro do ponto bate com o bairro do CEP em 84%. A maior parte das diferenças é grafia ("Osvaldo" e "Oswaldo Cruz") ou bairro vizinho, então juntar tabelas pelo nome do bairro causa erro.
5. **O status nem sempre diz se a árvore foi podada.** "Fechado com providências" e "com informação" (17%) não confirmam a poda. 15% foram fechados por "informações incorretas ou insuficientes", ou seja, por endereço ou descrição ruim, e não porque a árvore não precisava de poda.
6. **A coluna `dentro_prazo` está desatualizada** em 4,4% dos chamados: eles aparecem "No Prazo", mas as datas mostram que o prazo já passou. É melhor calcular o atraso pelas datas.
7. **Muitos chamados continuam abertos:** 13% no total, chegando a 27% dos abertos em 2026. Isso faz o tempo de atendimento recente parecer melhor do que é.
8. **As datas são coerentes entre si.** Nenhum chamado fecha antes de abrir e nenhum `id_chamado` se repete. Esse lado da base é confiável.

## Tabela de problemas

| Problema | Chamados | % da base | O que fazemos |
|---|---:|---:|---|
| Datas e códigos com tipo errado (texto/float) | 42.455 | 100 | Converter datas para data e códigos para texto |
| Pedidos que não são poda (remoção, risco, árvore caída) | 12.507 | 29,5 | Filtrar o subtipo para estudar poda |
| Mensagens de ouvidoria (reclamação, elogio...) | 5.740 | 13,5 | Não contar como pedido novo |
| Sem bairro ou sem logradouro | 1.057 | 2,5 | Usar a coordenada estimada ou descartar |
| Coordenada estimada pelo endereço | 14.898 | 35,1 | Usar com cautela em análises finas |
| Ponto fora da cidade | 3 | 0,0 | Descartar a coordenada |
| Número do imóvel 0 ou acima de 20.000 | 2.361 | 5,6 | Tratar como número desconhecido |
| Pedidos repetidos no mesmo endereço, dia e subtipo | 738 | 1,7 | Contar 1 por endereço e dia |
| Marcado "No Prazo", mas o prazo já passou | 1.875 | 4,4 | Calcular o atraso pelas datas |
| Ainda aberto (sem `data_fim`) | 5.698 | 13,4 | Tratar como "sem resposta" |
| Fechado "com providências" ou "com informação" | 7.116 | 16,8 | Não contar como poda feita |
| Fechado por informação insuficiente | 6.184 | 14,6 | Separar de "não precisava de poda" |

## Tipos dos dados

| Tipo certo | Colunas |
|---|---|
| Data/hora | `data_inicio`, `data_fim`, `data_alvo_finalizacao`, `data_alvo_diagnostico`, `data_real_diagnostico`, `updated_at`, `extracted_at`, `data_particao` |
| Float (decimal) | `latitude`, `longitude`, `latitude_usada`, `longitude_usada`, `dist_km_ao_cep` |
| Inteiro | `numero_logradouro`, `tempo_prazo`, `reclamacoes` |
| Código (guardado como texto) | `id_chamado`, `id_bairro`, `id_logradouro`, `id_subtipo`, `cd_setor`, `cep` e os demais `id_*` |
| Categoria (texto) | `categoria`, `subtipo`, `status`, `situacao`, `tipo_situacao`, `dentro_prazo`, `nome_unidade_organizacional` |
| Lógico | `unidade_organizacional_ouvidoria` |

Colunas quase sempre vazias, que não servem para análise: `data_alvo_diagnostico` (84% vazia), `data_real_diagnostico` (82%) e `justificativa_status` (81%; só é preenchida em "Sem possibilidade de atendimento").

## Outros achados do grupo (base completa no BigQuery)

- A chuva do Alerta Rio tem linhas duplicadas entre fev e mai/2024 e não existe para 2025.
- Bairros de favela (Maré, Rocinha, Jacarezinho, Alemão) fazem muito menos chamados do que o esperado pela população e pelas árvores. Isso é **viés de reporte**, e não falta de árvore.
