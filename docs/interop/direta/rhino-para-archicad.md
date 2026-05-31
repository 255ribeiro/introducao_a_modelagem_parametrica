# Interoperabilidade Rhino 8 → ArchiCAD 29
### Fluxo direto sem Speckle, Rhino.Inside ou Grasshopper-ArchiCAD Connection

---

## 1. Visão geral dos formatos de troca

| Formato | Como chega no ArchiCAD 29 | Recomendado para |
|---------|---------------------------|-----------------|
| **3DM** (nativo Rhino) | Objeto GDL na Biblioteca Embutida | **Primeira opção — sem conversão intermediária** |
| **DWG 3D com ACIS solids** | Morph editável | Geometria que precisa de edição direta no ArchiCAD |
| **SAT / ACIS** | Morph editável | Alternativa ao DWG com boa fidelidade NURBS |
| **OBJ / STL** | Morph (malha triangulada) | Apenas visualização e renderização |

A escolha entre `.3dm` e DWG/SAT depende do uso: `.3dm` é mais simples e direto; DWG/SAT gera Morphs editáveis quando é necessário manipular a geometria dentro do ArchiCAD.

---

## 2. Importação direta de .3DM (primeira opção)

### 2.1 Como funciona no ArchiCAD 29

O `.3dm` é lido nativamente pelo ArchiCAD 29 sem plugin adicional. A geometria é convertida em **objeto GDL** — um objeto de biblioteca paramétrico, não editável geometricamente dentro do ArchiCAD, mas com controle de segmentação ajustável. O objeto é armazenado na **Biblioteca Embutida** do projeto.

Cada sólido ou polissuperfície do Rhino vira um objeto GDL separado por camada.

> ⚠️ **Diferença importante do guia anterior:** o `.3dm` **não** entra como Morph. Entra como objeto GDL. Morphs só surgem quando a importação é feita via DWG ou SAT (seção 3).

### 2.2 Preparação no Rhino 8 (menus em inglês)

1. Organize os objetos por **layers** com nomes descritivos:
   ```
   PAREDES
   COBERTURAS
   LAJES
   PILARES
   CONTEXTO
   TERRENO
   ```
2. Confirme as unidades: `File > Properties > Units` — use **milímetros** ou **metros** conforme o template do ArchiCAD.
3. Elimine geometria auxiliar (pontos, linhas de construção) ou isole-a em layers separados.
4. Salve normalmente: `File > Save` (`.3dm`). Não é necessário exportar.

### 2.3 Mesclar o .3dm no ArchiCAD 29

1. `Arquivo > Interoperabilidade > Mesclar…`
2. Na caixa de seleção, mude o filtro para **Modelo 3D Rhino (*.3dm)**.
3. Selecione o arquivo e clique em **Abrir**.
4. Clique em **Opções** para acessar as configurações de importação:

| Parâmetro | Configuração recomendada |
|-----------|--------------------------|
| **Segmentação de geometria curva** | Ajuste com o slider — mais à direita = mais suave, mais pesado |
| **Cozer geometria curva** (Bake) | Marque para fixar a segmentação e ganhar desempenho e snap; desmarque se quiser ajustar depois |
| **Importar elementos ocultos** | Desmarque, a menos que precise de geometria escondida no Rhino |
| **Estrutura de camadas** | "Manter estrutura original de camadas" — preserva os layers do Rhino |
| **Superfícies (cores)** | "Importar cores Rhino como Superfícies" — cria materiais automáticos por cor |

5. Clique em **OK** e depois em **Mesclar**.

Os objetos GDL gerados ficam na **Biblioteca Embutida**, pasta separada por arquivo Rhino, com subpastas por layer.

### 2.4 Colocar os objetos GDL na planta

Após o Mesclar, os objetos GDL estão na biblioteca mas **ainda precisam ser colocados na planta**:

- Use a ferramenta **Objeto** (`O`) e localize os objetos importados na Biblioteca Embutida.
- Ou o ArchiCAD os coloca automaticamente na origem do projeto se você usou `Arquivo > Abrir` (não Mesclar).

### 2.5 Hotlink de .3dm — atualização automática

Esta é a grande vantagem do `.3dm` sobre DWG/SAT: é possível criar uma **Associação (Hotlink)** que atualiza automaticamente quando o arquivo Rhino é modificado.

1. `Arquivo > Conteúdo Externo > Colocar Associação…`
2. No diálogo **Novo Módulo Associado**, escolha o tipo de arquivo **Modelo 3D Rhino (*.3dm)**.
3. Clique em **Opções** para definir segmentação e camadas (mesmas opções do Mesclar).
4. Selecione o arquivo `.3dm` e clique em **Selecionar**.
5. Posicione o módulo na planta.

Para atualizar após alterações no Rhino:
- `Arquivo > Conteúdo Externo > Gerenciador de Módulos Associados…`
- Selecione o módulo Rhino e clique em **Atualizar**.

> Esta é a forma mais eficiente de trabalhar com modelos Rhino que evoluem ao longo do projeto — sem precisar deletar e reimportar manualmente.

### 2.6 Limitações do objeto GDL importado de .3dm

- **Não editável geometricamente** dentro do ArchiCAD — para modificar a forma, edite no Rhino e atualize.
- **Sem parâmetros BIM** nativos (área, volume em schedules). Para isso, converta para Morph (veja seção 4).
- **Texturas não transferidas** — aplique materiais no ArchiCAD nas configurações do objeto.
- **Segmentação fixa após Bake** — se marcou "Cozer geometria curva", não é possível ajustar depois.
- **Blocos aninhados** (blocos dentro de blocos) podem chegar desagrupados.

---

## 3. Alternativa: DWG 3D ou SAT → Morph editável

Use esta rota quando precisar editar a geometria diretamente no ArchiCAD ou convertê-la em elementos BIM.

### 3.1 Exportar como DWG 3D no Rhino 8

1. `File > Export Selected` → **AutoCAD DWG (*.dwg)**.
2. Na caixa de opções:
   - **Export surfaces as:** ACIS solids — garante NURBS preservadas, sem triangulação.
   - **Export open curves:** Yes.
   - Versão: **DWG 2010 ou 2013**.
3. Confirme as unidades.

### 3.2 Exportar como SAT no Rhino 8

1. `File > Export Selected` → **ACIS (*.sat)**.
2. Versão ACIS: 7 ou superior.

### 3.3 Mesclar DWG ou SAT no ArchiCAD 29

1. `Arquivo > Interoperabilidade > Mesclar…` → selecione o DWG ou SAT.
2. Na caixa de diálogo:
   - **Escala:** confirme unidades (mm → mm ou m → m).
   - **Mapeamento de camadas:** Manter Camadas Originais.
   - **Conversão de conteúdo 3D:** Criar Elementos 3D.
   - **História de destino:** pavimento correto.

A geometria entra como **Morph** — elemento nativo editável do ArchiCAD.

---

## 4. Fazer os objetos GDL e Morphs aparecerem nas plantas

### 4.1 Objetos GDL (.3dm importado)

Objetos GDL do Rhino exibem automaticamente uma **projeção 2D de cima** na planta — gerada pelo script GDL interno usando o comando `project2`. Esta projeção aparece por padrão, mas pode ser refinada:

1. Selecione o objeto → `Ctrl+T` → **Configurações do Objeto**.
2. Na aba **Parâmetros Personalizados** (específicos do objeto Rhino):
   - **Mostrar arestas de face (Show Face Edges):** ative para ver as arestas internas das superfícies na planta 2D.
   - **Pontos de apoio no bounding box:** útil para cotar objetos ortogonais.
3. Na aba **Planta Baixa e Corte**:
   - **Caneta de linha de projeção:** controla a espessura das linhas do objeto na planta.
   - **Tipo e caneta de trama:** define hachura quando o objeto é cortado.

### 4.2 Morphs (DWG/SAT importado)

Para Morphs, a configuração de visibilidade em planta é mais manual:

1. Selecione o(s) Morph(s) → `Ctrl+T` → **Configurações do Elemento**.
2. Na aba **Planta Baixa e Corte**:

| Parâmetro | Configuração recomendada |
|-----------|--------------------------|
| **Exibir em Histórias** | Todas as histórias relevantes |
| **Projeção** | Projetado com Vista Superior |
| **Mostrar contornos** | Ativado |
| **Mostrar preenchimento de corte** | Ativado |

**"Projetado com Vista Superior"** faz o ArchiCAD exibir a projeção ortogonal de cima do Morph na planta, independentemente de a linha de corte do pavimento atravessar o elemento — ideal para coberturas, volumes de fachada e qualquer geometria acima da linha de corte.

### 4.3 Verificar altura e posicionamento

Se o elemento não aparece na planta:

1. `Ctrl+T` → verifique **Elevação (base)** — deve estar no nível correto.
2. `Opções > Preferências do Projeto > Elementos de Construção` — verifique o **Plano de Corte** (padrão: 1,10 m acima do nível do pavimento).
3. A história de destino deve corresponder ao pavimento da vista.

---

## 5. Fazer os elementos aparecerem nos cortes

### 5.1 Objetos GDL

Objetos GDL do Rhino aparecem automaticamente em cortes e elevações como projeção das suas faces visíveis. Não é necessária configuração adicional além da visibilidade de camada.

### 5.2 Morphs

Nas **Configurações do Elemento**, aba **Planta Baixa e Corte**:

- **Exibir em Cortes/Elevações:** ative.
- **Projeção em Corte:** escolha **Mostrar Projetado** para que a geometria atrás do plano de corte apareça como linhas de fundo.
- **Preenchimento de Corte:** define a hachura quando o plano corta o Morph — atribua um material.
- **Caneta de Contorno:** espessura da linha do contorno cortado.
- **Caneta de Projeção:** espessura da linha do contorno projetado.

---

## 6. Fazer os elementos aparecerem nas elevações

### 6.1 Configuração

Para Morphs, nas **Configurações do Elemento**, aba **Planta Baixa e Corte**:
- **Exibir em Cortes/Elevações:** ativado.

Para objetos GDL (.3dm), a exibição em elevações é automática.

### 6.2 Qualidade da representação

- Com `.3dm` e DWG/SAT ACIS solids, superfícies curvas aparecem com boa qualidade — a segmentação controla a suavidade.
- Com DWG exportado como malha (não ACIS), a triangulação aparece nas linhas de elevação. Prefira sempre ACIS solids.
- Para objetos GDL, ajuste a **Segmentação** nas configurações do objeto para equilibrar suavidade e desempenho.

---

## 7. Controle de visibilidade por Camada e Combinação de Camadas

Após o Mesclar, as camadas do Rhino ficam disponíveis no **Gerenciador de Camadas**:

1. `Opções > Atributos de Elementos > Camadas` (ou `Ctrl+L`).
2. As camadas do Rhino chegam com o sufixo "Rhino" (ex.: `COBERTURAS Rhino`).
3. Crie **Combinações de Camadas** por tipo de vista:
   - Planta de trabalho: camada de contexto desativada.
   - Renderização: todas as camadas ativas.
   - Cortes técnicos: apenas camadas de envoltória e estrutura.

---

## 8. Converter objeto GDL em Morph (opcional)

Se precisar editar a geometria do objeto GDL importado de `.3dm` diretamente no ArchiCAD:

1. Selecione o objeto GDL.
2. Clique com o botão direito → **Converter em Morph** (ou `Editar > Reformatar > Converter Seleção em Morph`).

O Morph resultante é editável mas perde a ligação com o arquivo Rhino original.

---

## 9. Ciclo de revisão — comparativo dos fluxos

| Método de importação | Atualização quando Rhino muda |
|----------------------|-------------------------------|
| **Hotlink .3dm** | `Gerenciador de Módulos Associados > Atualizar` — automático |
| **Mesclar .3dm** | Delete e re-mescla manualmente |
| **Mesclar DWG/SAT** | Delete e re-mescla manualmente |

> Para projetos com iterações frequentes no Rhino, o **Hotlink .3dm** é claramente o melhor fluxo.

---

## 10. Fluxo resumido

```
Rhino 8 (inglês)                        ArchiCAD 29 (português)
────────────────                         ───────────────────────
Organizar por layers nomeados      →     Camadas com sufixo "Rhino" no Mesclar
File > Properties > Units (mm/m)   →     Unidades explícitas nas opções
File > Save (.3dm)                 →     Arquivo > Interoperabilidade > Mesclar
                                          → Objeto GDL na Biblioteca Embutida
                                          → Projeção 2D automática na planta
                                          → Show Face Edges para arestas internas
                                   OU
                                         Arquivo > Conteúdo Externo > Colocar Associação
                                          → Hotlink .3dm atualizável
──────────────────────────────────────────────────────────────────
File > Export Selected > DWG           Arquivo > Interoperabilidade > Mesclar
  (ACIS solids, versão 2010)     →      → Morph editável
                                          Ctrl+T > Projetado com Vista Superior
                                          Ctrl+T > Exibir em Cortes/Elevações
```

---

## 11. Boas práticas

- Use **Hotlink .3dm** para projetos com iterações frequentes — é o único método com atualização automática.
- Use **Mesclar .3dm** para geometria de referência estável, como contexto urbano ou topografia.
- Use **DWG/SAT → Morph** quando precisar editar a geometria diretamente no ArchiCAD.
- Confirme as unidades em `File > Properties > Units` no Rhino antes de salvar — é a causa mais comum de escala errada após o Mesclar.
- Ative **"Cozer geometria curva"** na importação quando o desempenho for prioridade — a navegação é mais rápida com geometria baked.
- Desative **"Cozer geometria curva"** quando a suavidade das curvas ainda puder ser ajustada no ArchiCAD — permite refinar depois sem precisar reimportar.
- Para geometria de contexto urbano, mantenha como objeto GDL em camada visível apenas em 3D — reduz o peso do arquivo e não polui as vistas técnicas.
