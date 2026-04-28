# Grasshopper

## O que é o Grasshopper?

O **Grasshopper** é um editor de programação visual (VPL — *Visual Programming Language*) integrado ao **Rhinoceros 3D** (Rhino). Desenvolvido por David Rutten na empresa **Robert McNeel & Associates**, o Grasshopper permite criar geometrias complexas e modelos paramétricos por meio de um fluxo de componentes visuais conectados, sem a necessidade de escrever código textual.

A partir da versão **Rhino 6**, o Grasshopper passou a ser instalado nativamente junto ao Rhino, sem necessidade de instalação separada.

---

## Características Principais

- **Programação por nós (*nodes*):** a lógica é construída conectando componentes (chamados de *nodes* ou *components*) que representam operações matemáticas, geométricas e de controle de dados.
- **Parametrismo:** alterações em parâmetros de entrada propagam-se automaticamente por todo o modelo, atualizando a geometria em tempo real.
- **Integração com o Rhino:** a geometria gerada no Grasshopper é exibida diretamente na viewport do Rhino, com atualização dinâmica.
- **Extensível por plugins:** o ecossistema de plugins (como Karamba3D, Ladybug Tools, Kangaroo, entre outros) amplia as capacidades do Grasshopper para estruturas, análises ambientais, simulações físicas, fabricação digital e muito mais.

---

## Tipos de Dados

O Grasshopper opera com os seguintes tipos de dados principais:

- **Números** (inteiros e decimais)
- **Vetores e Planos**
- **Pontos e Curvas**
- **Superfícies e Sólidos (Breps)**
- **Malhas (*Meshes*)**
- **Textos (*Strings*)**
- **Booleanos** (verdadeiro/falso)

---

## Listas e Estrutura de Dados

Um dos conceitos fundamentais do Grasshopper é a manipulação de **listas** e **árvores de dados (*Data Trees*)**, que permitem operar sobre conjuntos de geometrias e valores de forma eficiente e escalável.

---

## Fluxo de Trabalho Típico

1. Definir parâmetros de entrada (sliders, painéis, geometria do Rhino)
2. Conectar componentes para transformar e gerar geometria
3. Visualizar o resultado na viewport do Rhino em tempo real
4. Ajustar parâmetros e iterar sobre o design
5. Confirmar (*bake*) a geometria no Rhino quando necessário

---

## Plugins Populares

| Plugin | Aplicação |
|---|---|
| **Karamba3D** | Análise estrutural |
| **Ladybug Tools** | Análise ambiental e de conforto |
| **Kangaroo** | Simulação física e otimização de forma |
| **Lunchbox** | Painelização e padrões geométricos |
| **Human** | Interface e interações avançadas |
| **Weaverbird** | Subdivisão e manipulação de malhas |

---

## Referências

- Site oficial do Rhino/Grasshopper: <a href="https://www.rhino3d.com/6/new/grasshopper/" target="_blank">rhino3d.com</a>
- Fórum da comunidade: <a href="https://discourse.mcneel.com/c/grasshopper/2" target="_blank">McNeel Discourse — Grasshopper</a>
- Documentação de componentes: <a href="https://grasshopperdocs.com/" target="_blank">grasshopperdocs.com</a>
