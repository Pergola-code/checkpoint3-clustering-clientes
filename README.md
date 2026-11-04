# Checkpoint 3 — Clustering e Segmentação de Clientes

**Disciplina:** Modelagem Linear para Aprendizado de Máquina
**Atividade:** K-Means, método Elbow e Silhouette Score

## Integrantes

| Nome | RM |
| --- | --- |
| Felipe Macedo | 570990 |

**Turma:** _preencher_

## Objetivo

Identificar perfis de compra em uma base de clientes de um e-commerce usando o algoritmo **K-Means**, justificar a quantidade de clusters com o **método Elbow** e o **Silhouette Score**, interpretar cada perfil e propor ações de relacionamento, discutindo as limitações da análise.

## Base de dados

Base **simulada** de 120 clientes fictícios (gerada com NumPy, semente 303), incorporada ao próprio notebook como texto CSV — não é preciso baixar nenhum arquivo externo. Os dados não foram coletados de pessoas ou empresas reais.

| Coluna | Significado | Uso |
| --- | --- | --- |
| `id_cliente` | Identificador fictício | Excluído do modelo |
| `gasto_total_12_meses` | Soma das compras nos últimos 12 meses (R$) | Valor |
| `quantidade_compras_12_meses` | Número de compras nos últimos 12 meses | Frequência |
| `dias_desde_ultima_compra` | Dias desde a compra mais recente | Recência |

A base não tem valores ausentes nem identificadores repetidos.

## Como executar

**Google Colab:** abra `Checkpoint_3_MLAM_Clustering_Clientes.ipynb` no Colab e use *Ambiente de execução → Executar tudo*.

**Localmente:**

```bash
pip install pandas numpy matplotlib scikit-learn jupyter
jupyter notebook Checkpoint_3_MLAM_Clustering_Clientes.ipynb
```

Em seguida, *Kernel → Restart & Run All*. Todos os modelos usam `random_state=42` e `n_init=10`, então os resultados são reproduzíveis no mesmo ambiente (versões diferentes do scikit-learn podem alterar a numeração dos clusters).

## Metodologia

1. Exploração dos dados (tipos, ausentes, duplicados, estatísticas e amplitudes).
2. Seleção das três variáveis numéricas e padronização com `StandardScaler` (média 0, desvio 1), para que o gasto em reais não domine as distâncias.
3. Método Elbow com k de 1 a 8.
4. Silhouette Score com k de 2 a 8.
5. Modelo final, visualizações em duas projeções 2D e tabela de perfis nas unidades originais.

## Principais resultados

| k | 1 | 2 | 3 | **4** | 5 | 6 | 7 | 8 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Inércia | 360,00 | 164,71 | 110,52 | **60,65** | 46,92 | 40,08 | 33,75 | 28,41 |
| Silhouette | — | 0,4842 | 0,4889 | **0,5526** | 0,5433 | 0,5037 | 0,4644 | 0,4771 |

**k escolhido: 4.** O cotovelo aparece em k = 4 (a queda de inércia passa de ≈ 50 para ≈ 14) e k = 4 também tem o maior Silhouette Score (0,5526). Nenhum cliente ficou com silhueta negativa.

| Cluster | Clientes | Gasto médio | Compras médias | Dias desde a última compra | Perfil | Ação proposta (hipótese) |
| --- | ---: | ---: | ---: | ---: | --- | --- |
| 0 | 28 | R$ 9.309,6 | 27,1 | 10,5 | Alto valor, frequentes e recentes | Fidelização e benefícios exclusivos |
| 1 | 30 | R$ 6.741,4 | 6,3 | 75,6 | Ticket alto, compras esporádicas, recência distante | Reativação personalizada |
| 2 | 32 | R$ 2.861,7 | 22,7 | 15,0 | Frequentes e recentes, ticket baixo | Up-sell / cross-sell para elevar o ticket |
| 3 | 30 | R$ 833,3 | 4,1 | 102,4 | Baixo engajamento | Reativação automatizada de baixo custo |

## Limitações

- A base é simulada e pequena; os grupos tendem a ser mais nítidos do que em dados reais.
- Só há três variáveis agregadas: não há margem, categorias, satisfação ou data de cadastro, então gasto alto não comprova lucro e muitos dias sem comprar não comprovam abandono.
- O K-Means supõe grupos aproximadamente esféricos, é sensível a outliers e exige definir k previamente.

## Estrutura do repositório

```
├── Checkpoint_3_MLAM_Clustering_Clientes.ipynb   # notebook preenchido e executado
└── README.md
```
