# GTA VI vs GTA V: o hype medido na Wikipédia

Análise dos acessos diários às páginas em inglês da Wikipédia sobre **Grand Theft Auto V** e **Grand Theft Auto VI**, de 01/12/2023 a 18/11/2026, com os principais eventos do GTA VI (trailers, adiamentos, pré-venda) marcados nos gráficos.

Os acessos medem curiosidade, não vendas nem jogadores.

## Principais resultados

- O GTA VI chegou a **519.632 acessos em um único dia** (18/11/2026, véspera do lançamento).
- Isso é **52% acima do recorde anterior**, o Trailer 1 (341.519 acessos em 05/12/2023).
- No mesmo dia, o GTA V teve 81.349 acessos: o GTA VI teve **6,4 vezes** o interesse.
- No período inteiro, o GTA VI somou 12,1 milhões de acessos e o GTA V, 9,2 milhões.
- O GTA VI ficou à frente do GTA V em **569 dos 1.084 dias** (52%).

## Fonte dos dados

[API REST de pageviews da Wikimedia](https://wikimedia.org/api/rest_v1/), com estes parâmetros:

| Parâmetro | Valor |
|---|---|
| Projeto | `en.wikipedia` |
| Acesso | `all-access` |
| Agente | `user` (só leitores humanos, sem robôs) |
| Granularidade | `daily` |
| Páginas | `Grand_Theft_Auto_V` e `Grand_Theft_Auto_VI` |

A API devolve o timestamp no formato `YYYYMMDD00`, convertido com `pd.to_datetime(..., format="%Y%m%d00")`.

## Eventos marcados

| # | Evento | Data |
|---|---|---|
| 1 | Trailer 1 | 05/12/2023 |
| 2 | Adiamento 1 | 02/05/2025 |
| 3 | Trailer 2 | 06/05/2025 |
| 4 | Adiamento 2 | 06/11/2025 |
| 5 | Pré-venda | 25/06/2026 |
| 6 | Extended Look | 27/08/2026 |
| 7 | Pré-download | 10/11/2026 |
| 8 | Véspera do lançamento | 18/11/2026 |

O pico de cada evento é o maior valor do GTA VI entre o dia do evento e os dias seguintes, porque o efeito nem sempre aparece no mesmo dia.

## Como rodar

Requisitos: Python 3.9 ou superior.

```bash
pip install requests pandas matplotlib
python gta_pageviews.py
```

O script baixa os acessos das duas páginas e salva `gta_pageviews.csv` com as colunas `data`, `pagina` e `acessos` (2.168 linhas).

Para incluir dias mais recentes, troque a data final no script (formato `AAAAMMDD`). A Wikimedia fecha cada dia em UTC e costuma liberar o dado completo cerca de um dia depois.

## O que o projeto faz

1. **Coleta:** `requests` consulta a API da Wikimedia e salva um CSV.
2. **Preparo:** `pandas` converte as datas e organiza uma coluna por jogo, com um dia por linha.
3. **Gráficos:** `matplotlib` desenha as duas séries e marca os 8 eventos. A escala logarítmica escondia o tamanho real dos picos, então os gráficos usam escala linear.
4. **Zoom por evento:** um painel pequeno para cada evento, com 10 dias antes e 10 dias depois (`plt.subplots(2, 4)`), cada um com o seu próprio eixo y.

## Cuidados na leitura

- Acessos à Wikipédia em inglês refletem interesse em inglês, não o interesse global.
- O último dia do CSV é um dia completo; o dia em curso não entra.
- Notícias fora da lista de eventos podem explicar picos que aparecem sem evento marcado.

## Licença e créditos

Dados: Wikimedia Foundation, pela API pública de pageviews.
