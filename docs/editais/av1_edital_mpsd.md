# Atividade de Avaliação 01: Edital

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

1. Exercitar a modelagem paramétrica de volumes arquitetônicos em código;
2. Compreender o uso de variáveis e operações de modelagem na definição de formas edificadas;
3. Explorar o potencial das ferramentas de geração de imagem por Inteligência Artificial como instrumento de visualização e proposta arquitetônica.

---

<h4 style="background:lightblue">Orientações gerais</h4>

Os trabalhos podem ser feitos de forma **individual ou em grupos de até cinco (5) alunos**. O nome de todos os integrantes do grupo deve constar no início do código, como comentários ou célula MArkdown no caso de arquivos do google colab.

O código deve ser comentado, demonstrando o entendimento de cada etapa do processo de modelagem.

---

<h4 style="background:lightblue">Descrição da atividade</h4>

### 1. Modelagem do Volume Edificado

Utilizando a biblioteca **CadQuery** em Python, modele o **volume básico de uma edificação de múltiplos pavimentos**, com no mínimo **30 andares**.

O modelo deve ser inteiramente **paramétrico**: todas as dimensões (largura, profundidade, altura por pavimento, número de pavimentos, recuos, etc.) devem ser definidas como variáveis no código.

**Requisitos mínimos do modelo:**

- Implantação com dimensões reais (em metros), compatíveis com um edifício de alto padrão
- Número de pavimentos ≥ 30
- O volume deve refletir uma composição arquitetônica com intenção projetual — não apenas uma caixa única. Exemplos de recursos permitidos:
  - Variação volumétrica entre pavimentos (torre, embasamento, coroamento)
  - Recuos e balanços
  - Pátios ou vazios internos
  - Rotações ou inclinações de volumes
<!-- - O modelo deve utilizar ao menos **duas operações de modelagem** diferentes, à escolha do grupo. Exemplos: operações booleanas (união, subtração, interseção), extrusão (`extrude`), varredura (`sweep`), transição entre perfis (`loft`), entre outras disponíveis no CadQuery -->


---

### 2. Exportação do Modelo

Exporte o modelo final:

```python
cq.exporters.export(modelo, "edificio.step")
# ou
cq.exporters.export(modelo, "edificio.stl")
```

O arquivo exportado deve ser entregue junto com o código.

---

### 3. Capturas de Tela do Modelo

A partir do visualizador interativo, ou de imagens geradas pelos arquivos exportados abertos em softwares de modelagem CAD ou BIM, capture **três imagens** do modelo em ângulos distintos que melhor representem a volumetria proposta. Sugestões de ângulos:

- Vista em perspectiva isométrica (ângulo de conjunto) — **obrigatória**
- Vistas internas de pátios ou aberturas
- Vistas com detalhe do coroamento ou embasamento

As imagens devem estar em boa resolução e apresentar o modelo de forma clara. O grupo pode escolher livremente a origem das imagens:

- **cadquery-simpleViewer** — diretamente no notebook, usando `visible_axes=None` e plano de chão quando apropriado
- **Software CAD ou BIM** — importando o arquivo `.step` exportado no Rhino, FreeCAD, Fusion 360, Revit ou similar e realizando renders ou capturas a partir desses ambientes

> 💡 **Dica**: o visualizador do cadquery-simpleViewer é recomendado para **orientar a modelagem** durante o desenvolvimento. Para a geração das imagens finais, abrir o `.step` em um software dedicado pode contornar eventuais problemas de tessellação e produzir imagens com melhor qualidade visual para submissão à IA.

---

### 4. Geração de Imagens com Inteligência Artificial

Utilizando uma ferramenta de geração de imagem por IA **à escolha do grupo** (exemplos: Gemini, Midjourney, Adobe Firefly, DALL·E, Stable Diffusion, Leonardo.ai, Ideogram, etc.), submeta as três capturas do modelo como referência de forma e proporção.

O **prompt** enviado à IA deve orientar a ferramenta a:

- **Preservar a forma e as dimensões gerais** do volume modelado
- Acrescentar **materialidade arquitetônica** (vidro, concreto, alumínio, etc.)
- Incluir **vegetação** (coberturas verdes, jardins verticais, praças no entorno)
- Inserir **contexto urbano** (ruas, calçadas, outros edifícios, céu)
- Gerar uma **proposta arquitetônica coerente** com o volume de partida

Cada grupo deve gerar pelo menos **três imagens finais** (uma por ângulo capturado).

> 💡 **Dica**: imagens submetidas como referência de composição (*image-to-image* ou *image prompt*) tendem a preservar melhor a volumetria do modelo. Ferramentas que aceitam imagem como entrada são recomendadas.

---

<h4 style="background:lightblue">Itens da entrega</h4>

| #   | Item                                                                    | Formato                            |
| --- | ----------------------------------------------------------------------- | ---------------------------------- |
| 1   | Código Python do modelo                                                 | `.ipynb` ou `.py`                  |
| 2   | Modelo exportado                                                        | `edificio.step`                    |
| 3   | Capturas do modelo (3 imagens) — do visualizador ou de software CAD/BIM | `.png` ou `.jpg`                   |
| 4   | Imagens submetidas à IA (3 imagens + prompts utilizados)                | `.png` / `.jpg` + `.txt` ou `.md`  |
| 5   | Imagens geradas pela IA (mínimo 3)                                      | `.png` ou `.jpg`                   |
| 6   | Banner de apresentação da proposta                                      | `.pdf` ou `.png`, formato A2 ou A1 |

### Banner de apresentação

O banner deve conter, no mínimo:

- Nome do projeto e dos integrantes do grupo
- Perspectivas geradas pela IA (ao menos duas)
- Uma imagem do modelo CadQuery (wireframe ou renderizado)
- Breve memorial descritivo da proposta (até 150 palavras)
- Dados quantitativos: número de pavimentos, área do pavimento tipo, área total construída

---

<h4 style="background:lightblue">Critérios de avaliação</h4>

| Critério                                                                          | Peso |
| --------------------------------------------------------------------------------- | ---- |
| Qualidade e complexidade do modelo CadQuery                                       | 30%  |
| Uso correto de parametrização e diversidade das operações de modelagem utilizadas | 20%  |
| Qualidade das capturas e coerência com o modelo                                   | 15%  |
| Qualidade das imagens geradas pela IA e aderência ao volume                       | 20%  |
| Banner — clareza, organização e completude                                        | 15%  |

---


<h4 style="background:lightblue">Formato da entrega e envio</h4>

Os trabalhos devem ser enviados em **arquivo compactadao** (zip, rar, 7z, tar.gz ...) pelo Canvas da disciplina, organizados na seguinte estrutura de pastas:

```
grupo_XX/
├── codigo/
│   └── edificio.ipynb
├── modelo/
│   └── edificio.step
├── capturas/
│   ├── vista_01.png
│   ├── vista_02.png
│   └── vista_03.png
├── ia/
│   ├── prompts.txt
│   └── geradas/
└── banner/
    └── banner_grupo_XX.pdf
```

---

#### **Data de entrega: 13 de abril de 2026**

#### A entrega fora do prazo terá descontos na nota.

---
