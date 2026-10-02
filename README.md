# It's Raining Cats — Limpeza e Análise de Dados

Projeto de limpeza e análise exploratória de um dataset propositalmente sujo sobre gatos de três raças: **Angora**, **Ragdoll** e **Maine Coon**. O objetivo foi tratar todos os problemas de qualidade dos dados e responder perguntas sobre características e comportamento desses animais por meio de visualizações.

---

## Estrutura do Projeto

```
 projeto
 ┣ It's_raining_cats.ipynb     # Notebook principal
 ┣ cat_breeds_dirty.csv        # Dataset original (sujo)
 ┗ README.md
```

---

## Dataset

- **1.103 registros** | **17 colunas**
- Três raças: Angora, Ragdoll e Maine Coon
- Cinco países: EUA, França, UK, Alemanha e Canadá
- Dataset propositalmente preenchido com erros para fins de prática de limpeza de dados

### Colunas

| Coluna | Descrição |
|--------|-----------|
| `Breed` | Raça do gato |
| `Age_in_years` / `Age_in_months` | Idade em anos e meses |
| `Gender` | Sexo (male/female) |
| `Neutered_or_spayed` | Castrado ou não |
| `Body_length` | Comprimento do corpo (cm) |
| `Weight` | Peso (kg) |
| `Fur_colour_dominant` | Cor predominante do pelo |
| `Fur_pattern` | Padrão do pelo |
| `Eye_colour` | Cor dos olhos |
| `Allowed_outdoor` | Tem acesso à rua |
| `Preferred_food` | Tipo de ração preferida (wet/dry) |
| `Owner_play_time_minutes` | Minutos de brincadeira com o dono por dia |
| `Sleep_time_hours` | Horas de sono por dia |
| `Country` | País de origem do registro |
| `Latitude` / `Longitude` | Coordenadas geográficas |

---

## Limpeza dos Dados

### Problemas encontrados e soluções aplicadas

#### 1. Valores nulos
O dataset apresentava nulos em todas as 17 colunas:
- **Colunas categóricas** → preenchidas com a **moda** (valor mais frequente)
- **Colunas numéricas** → preenchidas com a **mediana** (mais robusta que a média para dados com outliers)

#### 2. Erros de digitação na coluna `Breed`

| Valor encontrado | Corrigido para |
|-----------------|----------------|
| Ankora, Angorra, angora | **Angora** |
| My coon, maine coon, Maine loon | **Maine coon** |
| rack doll, wrack doll, ragdoll | **Ragdoll** |

#### 3. Valores sem sentido em colunas categóricas

| Coluna | Valor encontrado | Tratamento |
|--------|-----------------|------------|
| `Fur_colour_dominant` | *"what does it mean dominant?"* | → `NaN` |
| `Fur_pattern` | *"dirty"* | → `NaN` |
| `Eye_colour` | *"cute"*, *"I dont know. Its pretty!"* | → `NaN` |
| `Preferred_food` | *"a lot of food"* | → `NaN` |
| `Allowed_outdoor` | *"I never allow my kitty outside!!!!!"*, *"I dont allow her outside. I'm a responsible owner"* | → `FALSE` |
| `Country` | *"La France!!!!"*, *"Vive la France!"*, *"france"* | → `France` |
| `Country` | *"my country"*, *"with me"*, *"where I live"*, *"Nan"* | → `NaN` |

#### 4. Valores numéricos inválidos
- **Idades negativas** em `Age_in_years` e `Age_in_months` → convertidas para positivo com `.abs()`
- **Peso absurdo de 320kg** → corrigido para **3.20kg** (erro de digitação com vírgula)
- **`Age_in_years`** arredondada para duas casas decimais com `.round(2)`

#### 5. Registros duplicados
Foram identificados **32 registros duplicados**, todos da raça Angora, concentrados no final do dataset.

#### 6. Strings "NaN" virando texto
Após substituições, algumas colunas ficaram com o texto `"NaN"` ou `"nan"` ao invés de valor nulo real convertidas para `np.nan` e preenchidas com a moda.

---

## Insights das Visualizações

### Comprimento do corpo por raça
O **Maine Coon** confirmou ser a maior raça:

| Raça | Comprimento médio |
|------|------------------|
| Maine Coon | **58.4 cm** |
| Ragdoll | 39.7 cm |
| Angora | 35.6 cm |

<img width="955" height="550" alt="image" src="https://github.com/user-attachments/assets/464b7a2b-323e-432c-9723-9bef9d166e54" />


Resultado condizente com a realidade o Maine Coon é uma das maiores raças domésticas do mundo.

### Raça e país predominantes
- **Ragdoll** é a raça mais registrada (508 gatos), seguido do Maine Coon (306) e da Angora (289)
- **EUA** lideram em registros (718), seguidos por França (113), UK (110), Alemanha (88) e Canadá (74)

<img width="1192" height="487" alt="image" src="https://github.com/user-attachments/assets/6e4ea763-568c-4c4d-9f0f-cfb1fba09a5d" />


### Cor do pelo por sexo
- **Red/cream** é fortemente predominante em **machos** (117 machos vs 31 fêmeas) a diferença mais expressiva do dataset
- **Branco** é levemente mais comum em **machos** (149 vs 132)
- **Seal** e **preto** têm distribuição equilibrada, com leve predominância feminina no seal (164 vs 142)

<img width="1151" height="556" alt="image" src="https://github.com/user-attachments/assets/3d19315f-ee13-4225-bed8-a9164b3f8c0d" />


### Cor do pelo por raça
Os dados revelaram padrões muito bem definidos por raça:
- **Angora** → predominantemente **branco** (220 de 289 gatos)
- **Ragdoll** → predominantemente **seal** (305 de 508 gatos) faz sentido pois o padrão colorpoint é característico da raça
- **Maine Coon** → maior variedade de cores, com destaque para **preto** (155) e **brown/chocolate** (74)

<img width="1155" height="549" alt="image" src="https://github.com/user-attachments/assets/ac923fb4-e1a7-4602-b273-676575e2e444" />


### Brincadeira e peso
Correlação negativa de **-0.26** gatos que brincam menos com o dono tendem a pesar mais. Relação moderada, pois raça e idade também influenciam o peso.

<img width="1054" height="549" alt="image" src="https://github.com/user-attachments/assets/7d1d9348-4aae-4cc2-8120-0b1b04d9b378" />


### Idade, peso e brincadeira
- **Idade x Peso:** correlação de **+0.47** a mais forte entre as analisadas. Gatos mais velhos tendem a pesar mais de forma consistente
- **Idade x Brincadeira:** correlação de **-0.26** gatos mais velhos brincam menos com o dono, esperado pois filhotes são naturalmente mais ativos

<img width="1352" height="557" alt="image" src="https://github.com/user-attachments/assets/d0f2cf4c-946f-4cd9-b1ae-1b7139b5d497" />


---

## Tecnologias Utilizadas

- Python 3
- Pandas
- NumPy
- Matplotlib
- Seaborn

---

## Como Executar

1. Clone o repositório
```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
```

2. Instale as dependências
```bash
pip install pandas numpy matplotlib seaborn
```

3. Abra o notebook
```bash
jupyter notebook It_s_raining_cats.ipynb
```

> Certifique-se de que o arquivo `cat_breeds_dirty.csv` está na mesma pasta do notebook antes de rodar.
