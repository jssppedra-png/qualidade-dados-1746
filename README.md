# Qualidade dos dados do 1746: chamados de manejo de árvores (Rio de Janeiro)

Repositório de josé no projeto de extensão **Ciência de Dados para Cidades Inteligentes** (Instituto de Matemática, UFRJ).

O objetivo do projeto, no ciclo 1, é **prever quantos pedidos de poda** cada rua do Rio terá em cada mês, usando os chamados da Central 1746. Antes de modelar, este repositório faz o **levantamento e a checagem de qualidade** da base.

## O que tem aqui

| Arquivo | O que é |
|---|---|
| [`SUMARIO.md`](SUMARIO.md) | Resumo dos problemas encontrados e o que fazer com cada um |
| [`notebooks/01_qualidade_dados.ipynb`](notebooks/01_qualidade_dados.ipynb) | Notebook com toda a análise: objetivo, tipos dos dados, qualidade e discrepâncias |
| [`dados/LEIAME.md`](dados/LEIAME.md) | De onde vêm os dados e como obtê-los |

## Como rodar

1. Abra o notebook no Google Colab (*Arquivo → Abrir notebook → GitHub*, cole o link deste repositório).
2. Clique em *Ambiente de execução → Executar tudo*.
3. Quando o Colab pedir, envie os arquivos CSV descritos em [`dados/LEIAME.md`](dados/LEIAME.md).

Bibliotecas usadas: `pandas`, `numpy`, `matplotlib` (todas já vêm no Colab).
