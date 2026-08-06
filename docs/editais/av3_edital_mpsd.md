# Atividade de Avaliação 03: Edital

---

<div style="align: top;">

<span style="float: left;">
<img src="./figs/Universidade_SENAI_CIMATEC.png" width="150">
</span>
<span style="float: right;"><br>
CENTRO UNIVERSITÁRIO SENAI CIMATEC <br>
CURSO DE ARQUITETURA E URBANISMO
</span>

</div>

<br><br><br><br><br><br>

<div>
    <span style="float: left;">Docente: Prof. Dr. Fernando Ferraz Ribeiro</span>
    <span style="float: right;">Semestre: 2026.1</span>
</div>

<br>

---

<h4 style="background:lightblue">Objetivo da avaliação</h4>

1. Compreender o arquivo climático EPW como fonte de dados paramétricos para análise ambiental;
2. Aplicar ferramentas de análise de radiação solar e sombreamento sobre o lote e seu entorno;
3. Explorar a influência da orientação de uma edificação no desempenho de suas fachadas.

---

<h4 style="background:lightblue">Orientações gerais</h4>

Os trabalhos podem ser feitos de forma **individual ou em grupos de até cinco (3) alunos**. O nome de todos os integrantes deve constar no arquivo de entrega.

As três atividades são **progressivas e complementares** — a Atividade 1 gera os dados climáticos que fundamentam as análises seguintes, a Atividade 2 modela o lote e aplica análises de radiação e sombreamento, e a Atividade 3 insere um edifício nesse mesmo lote. Todas as análises devem ser realizadas dentro do ambiente **Rhino + Grasshopper** com o plugin **Ladybug Tools** instalado.

> 💡 Os arquivos EPW para qualquer cidade do mundo podem ser obtidos gratuitamente em [Climate.OneBuilding.Org](https://climate.onebuilding.org/) ou no [EnergyPlus Weather Data](https://energyplus.net/weather).

---

<h4 style="background:lightblue">Descrição das atividades</h4>

### Atividade 1 — Estudo Climático Comparativo entre Duas Cidades

Importe os arquivos EPW de **duas cidades** à sua escolha — **uma delas deve ser obrigatoriamente brasileira** — e gere, para cada uma, os seguintes gráficos usando os componentes do Ladybug:

- **Heatmap de temperatura** ao longo do ano — componente `LB Hourly Plot`
- **Rosa dos ventos** — componente `LB Wind Rose`
- **Diagrama do percurso solar** — componente `LB SunPath`

O objetivo é entender como o arquivo EPW organiza dados climáticos horários e como esses dados variam entre climas distintos.

**Entregáveis desta atividade:**

- Canvas Grasshopper com todos os componentes conectados (arquivo `.gh`)
- Capturas de tela de todos os gráficos gerados para as **duas cidades** e um parágrafo comparativo descrevendo as diferenças climáticas observadas e suas implicações para o projeto arquitetônico — **inseridos no relatório em PDF** (ver seção "Relatório em PDF")

---

### Atividade 2 — Análise de Radiação e Sombreamento no Lote

Modele no Rhino um **lote fictício** com edificações vizinhas ao redor (volumes simples representando o entorno construído). Não é necessário reproduzir um lote real — o objetivo é criar uma situação plausível de implantação urbana.

Sobre a superfície do **terreno do lote**, aplique:

- **Análise de radiação acumulada anual** — componente `LB Incident Radiation`
- **Estudo de sombreamento** para duas datas-chave ao meio-dia solar:
  - Solstício de verão (21 de dezembro no hemisfério sul)
  - Solstício de inverno (21 de junho no hemisfério sul)
  - Componente `LB Shadow Study`

O arquivo EPW usado deve ser o da cidade **brasileira** escolhida na Atividade 1.

**Entregáveis desta atividade:**

- Arquivo Rhino com o modelo do lote e entorno (`.3dm`)
- Canvas Grasshopper com as análises conectadas (`.gh`)
- Captura do heatmap de radiação anual sobre o terreno e capturas das sombras projetadas no solstício de verão e no solstício de inverno (2 imagens) — **inseridas no relatório em PDF**

---

### Atividade 3 — Edifício no Lote: Radiação nas Fachadas por Rotação

Insira um **edifício simples** (volume paramétrico) no lote modelado na Atividade 2. O edifício deve ser posicionado com um parâmetro de **rotação** que permita testá-lo em diferentes orientações.

Aplique a análise de radiação acumulada anual (`LB Incident Radiation`) nas **fachadas e cobertura** do edifício para **pelo menos três rotações**, por exemplo: 0°, 45° e 90°.

O parâmetro de rotação deve ser controlado por um **slider** no Grasshopper — ao mover o slider, o edifício gira e a análise atualiza automaticamente.

**Entregáveis desta atividade:**

- Canvas Grasshopper atualizado com o edifício e o slider de rotação (`.gh`)
- Capturas do heatmap de radiação nas fachadas para cada uma das três rotações (3 imagens) e a análise comparativa — qual orientação resulta em menor radiação nas fachadas mais críticas (poente) e qual maximiza a captação solar na fachada norte, e por quê — **inseridas no relatório em PDF**

---

<h4 style="background:lightblue">Relatório em PDF</h4>

Todas as capturas de tela e todos os textos de análise/comparação das três atividades devem ser reunidos em **um único relatório em PDF**, com a seguinte estrutura mínima:

- **Capa**: identificação do grupo e de todos os integrantes
- **Seção 1 — Estudo Climático Comparativo**: gráficos (heatmap, rosa dos ventos, percurso solar) das duas cidades, lado a lado, seguidos do parágrafo comparativo
- **Seção 2 — Radiação e Sombreamento no Lote**: heatmap de radiação anual sobre o terreno e as capturas de sombreamento nos dois solstícios
- **Seção 3 — Edifício no Lote**: heatmaps de radiação nas fachadas para as três rotações e a análise comparativa das orientações

As imagens devem estar legíveis e identificadas (legenda indicando cidade, data ou ângulo de rotação, conforme o caso). Os arquivos `.gh` e `.3dm` continuam sendo entregues separadamente — o PDF concentra apenas as imagens e os textos interpretativos.

---

<h4 style="background:lightblue">Itens da entrega</h4>

| # | Item                                                                               | Formato |
| - | ---------------------------------------------------------------------------------- | ------- |
| 1 | Canvas Grasshopper com gráficos climáticos (Atividade 1)                           | `.gh`   |
| 2 | Modelo Rhino do lote e entorno (Atividade 2)                                       | `.3dm`  |
| 3 | Canvas Grasshopper com análises do lote (Atividade 2)                              | `.gh`   |
| 4 | Canvas Grasshopper com edifício e slider de rotação (Atividade 3)                  | `.gh`   |
| 5 | Relatório em PDF com todas as capturas e análises comparativas das três atividades | `.pdf`  |

---

<h4 style="background:lightblue">Critérios de avaliação</h4>

| Critério                                                                                        | Peso |
| ----------------------------------------------------------------------------------------------- | ---- |
| Atividade 1 — Qualidade e diversidade dos gráficos climáticos e clareza da análise comparativa | 25%  |
| Atividade 2 — Modelagem do lote e correto uso dos componentes de radiação e sombreamento        | 30%  |
| Atividade 3 — Parametrização da rotação e qualidade da análise comparativa das orientações      | 35%  |
| Organização geral dos arquivos e clareza dos textos interpretativos                             | 10%  |

---

<h4 style="background:lightblue">Formato da entrega e envio</h4>

Os trabalhos devem ser enviados em **arquivo compactado** (zip, rar, 7z, tar.gz ...) pelo Canvas da disciplina, organizados na seguinte estrutura de pastas:

```
grupo_XX/
├── ativ1_clima/
│   ├── clima_comparativo.gh
│   ├── cidade_A/
│   │   └── (capturas dos gráficos)
│   ├── cidade_B/
│       └── (capturas dos gráficos)
│   
├── ativ2_lote/
│   ├── lote_entorno.3dm
│   ├── analise_lote.gh
│   └── (capturas: radiação + sombras)
└── ativ3_rotacao/
|    ├── edificio_lote.gh
|   ├── rotacao_0deg.png
|   ├── rotacao_45deg.png
|   ├── rotacao_90deg.png
|__ Relatório
    |__ Relatório.pdf
```

---

#### **Data de entrega: Consulte o AVA da disciplina**

<!-- #### A entrega fora do prazo terá descontos na nota. -->

---
