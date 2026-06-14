# Modelos de Ruína do Jogador

Estudo do problema clássico da **Ruína do Jogador** modelado como um passeio
aleatório em uma cadeia de Markov com barreiras absorventes em `0` (ruína) e
`N` (vitória), com validação por simulação de Monte Carlo e uma aplicação a
dados financeiros reais.

## Scripts

| Arquivo | Descrição | Figuras geradas |
|---|---|---|
| `simulacao_ruina.R` | Estudo simulado: trajetórias, probabilidade de ruína e duração esperada — comparando resultados empíricos (Monte Carlo) com as fórmulas teóricas. | `fig_trajetorias.png`, `fig_ruin_prob.png`, `fig_duration.png` |
| `aplicacao_real.R` | Aplicação a dados reais da PETR4 (2020–2024): retornos diários são discretizados em alta (+1) / queda (−1), estima-se `p`, testa-se a hipótese de jogo justo e compara-se a ruína empírica com a teórica. | `fig_real_ruin.png`, `fig_real_traj.png` |

## Fundamentos

Para capital inicial `x`, total `N` e probabilidade de ganho `p` (`q = 1 − p`):

- **Probabilidade de ruína:** `ρ(x) = x/N` se `p = 0,5`; caso contrário
  `ρ(x) = (rˣ − rᴺ)/(1 − rᴺ)` com `r = q/p`.
- **Duração esperada:** `E[T] = x(N − x)` se `p = 0,5`; caso contrário
  `E[T] = (N·u(x) − x)/(p − q)`, onde `u(x)` é a probabilidade de vitória.

## Como executar

Requer **R (≥ 4.1)**. Os pacotes faltantes são instalados automaticamente.

```r
Rscript simulacao_ruina.R   # estudo simulado
Rscript aplicacao_real.R    # aplicação real (requer internet p/ baixar PETR4)
```

Dependências: `ggplot2`, `dplyr`, `scales` (script 1) e adicionalmente
`quantmod`, `zoo` (script 2).

## Otimizações aplicadas

- A simulação de Monte Carlo calcula **ruína e duração numa única varredura**,
  eliminando a execução duplicada da simulação.
- Funções teóricas **vetorizadas** sobre o vetor de capitais iniciais.
- **Função auxiliar de plotagem** reutilizada para os gráficos comparativos.
- Código comentado e enxuto, mantendo as figuras de saída idênticas.
