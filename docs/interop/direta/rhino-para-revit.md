# Interoperabilidade Rhino 8 → Revit 2026
### Fluxo direto sem Speckle, Rhino.Inside ou Grasshopper

---

## 1. Visão geral dos formatos de troca

| Formato | Transfere | Limitações |
|---------|-----------|------------|
| **SAT / ACIS** (`.sat`) | Sólidos e superfícies NURBS | Sem dados BIM; geometria "importada" |
| **DWG / DXF** | Linhas, sólidos, malhas | Perde curvatura NURBS complexa |
| **IFC** | Elementos com semântica BIM | Rhino não exporta IFC nativo sem plugin |
| **FBX** | Malhas trianguladas | Só visual; sem edição paramétrica |

Para geometria de trabalho (não apenas visualização), **SAT é o formato recomendado**.

---

## 2. Exportação do Rhino 8 (menus em inglês)

### 2.1 Exportar como SAT

1. Selecione os objetos desejados.
2. `File > Export Selected` → **ACIS (*.sat)**.
3. Versão ACIS: 7 ou superior (compatível com Revit 2026).
4. Confirme as unidades: `File > Properties > Units` — use **milímetros** ou **metros**.

> **Dica:** exporte por categoria em arquivos SAT separados (paredes, coberturas, estrutura). Facilita a gestão no Revit e a criação de elementos por face.

### 2.2 Exportar como DWG 3D

1. `File > Export Selected` → **AutoCAD DWG (*.dwg)**.
2. Na caixa de opções:
   - **Export surfaces as:** ACIS solids — garante NURBS preservadas.
   - **Export open curves:** Yes.
   - Versão: **DWG 2010 ou 2013**.
3. Confirme as unidades.

---

## 3. Importação no Revit 2026 (menus em português)

### 3.1 Importar vs. Vincular CAD

- `Inserir > Importar CAD` — importação permanente, incorporada no arquivo.
- `Inserir > Vincular CAD` — ligação externa; atualizável quando o arquivo SAT for substituído.

Para geometria que evolui no Rhino, use **Vincular CAD** — permite recarregar sem reimportar.

### 3.2 Configurações de importação críticas

| Parâmetro | Recomendação |
|-----------|-------------|
| **Cores** | Preservar ou Por camada |
| **Camadas/Níveis** | Todas ou seleção manual |
| **Unidades de importação** | Escolha explicitamente (milímetros/metros) — **nunca** "Detectar automaticamente" em SAT |
| **Posicionamento** | Manual ou Origem a origem |
| **Colocar em** | Nível específico |

> ⚠️ Se o modelo abrir em escala errada, o problema quase sempre é a unidade de importação. Desfaça (`Ctrl+Z`) e reimporte com a unidade correta.

---

## 4. O fluxo correto: SAT importado como Massa → elementos por face

Este é o ponto mais importante do guia. No Revit, a rota recomendada pela Autodesk para usar geometria externa como base de elementos BIM é:

**Geometria SAT → Família de Massa → Modelo por face (Parede, Telhado, Piso)**

Não existe um caminho direto de SAT para parede — a geometria passa por uma **Massa** (conceitual ou importada), e as faces dessa massa são convertidas em elementos construtivos.

---

## 5. Importar SAT dentro de uma Família de Massa

Este é o fluxo mais robusto para usar geometria Rhino como base de elementos BIM.

### 5.1 Criar uma Família de Massa com o SAT

1. No Revit, acesse `Arquivo > Novo > Família`.
2. Selecione o template **Massa.rft** e clique em **Abrir**.
3. Dentro do editor de família: `Inserir > Importar CAD`.
4. Selecione o arquivo SAT exportado do Rhino.
5. Configure unidades, posicionamento e camadas (mesmas recomendações da seção 3.2).
6. Clique em **Abrir** — o sólido SAT aparece no editor de massa.
7. Salve a família (`.rfa`) e feche o editor.
8. De volta ao projeto: `Inserir > Carregar Família` → selecione o `.rfa` criado.
9. Coloque a massa no projeto com a ferramenta **Componente** ou diretamente pelo Navegador de Projeto.

### 5.2 Por que usar Família de Massa e não importar SAT direto no projeto?

Importar o SAT diretamente no projeto gera uma geometria do tipo **Forma Direta** (Direct Shape) — útil para referência visual, mas as ferramentas "por face" funcionam de forma mais previsível e completa quando a geometria está dentro de uma **Família de Massa** ou de uma **Massa no Local**.

---

## 6. Criar Paredes a partir de Faces

### 6.1 Ativar a exibição de massas

Por padrão, massas ficam ocultas. Para visualizá-las:

- Aba **Massa e Terreno > Massa Conceitual > Exibir Massas por Vista e Tipo**.

### 6.2 Parede por Face

1. Aba **Massa e Terreno > Modelo por Face > Parede**.
2. Selecione o tipo de parede no seletor de tipo (barra superior).
3. Clique numa face da massa importada do Rhino.
4. O Revit cria uma parede paramétrica vinculada àquela face.
5. Defina o nível base e o nível topo nas propriedades.

**Quando funciona bem:**
- Faces planas inclinadas (paredes oblíquas, paredes em ângulo).
- Superfícies regladas simples (faces deriváveis de retas — cilíndricas, cônicas).
- Formas poliédricas angulosas.

**Quando tem limitações:**
- Superfícies de dupla curvatura (paraboloides, formas freeform complexas) — o Revit pode criar uma **Parede Cortina por Face** em vez de parede simples, subdividida em painéis.
- Faces muito pequenas ou adjacentes sem clearance suficiente.
- Faces que mudam de orientação abruptamente — o Revit pode rejeitar ou criar a parede espelhada.

### 6.3 Parede Cortina por Face

Para superfícies curvas complexas:

1. Aba **Massa e Terreno > Modelo por Face > Sistema de Parede Cortina**.
2. Clique nas faces desejadas.
3. O Revit cria um sistema de parede cortina que acompanha a geometria curva como painéis.

---

## 7. Criar Telhados a partir de Faces

### 7.1 Telhado por Face

1. Aba **Massa e Terreno > Modelo por Face > Telhado**.
2. Clique nas faces da massa que representam a cobertura.
3. Clique em **Criar Telhado**.

**Casos de uso reais:**
- Coberturas inclinadas irregulares modeladas livremente no Rhino.
- Coberturas com uma curvatura (cilíndricas).
- Formas de cobertura angulosas que seriam trabalhosas nas ferramentas nativas.

**Limitações:**
- Coberturas de dupla curvatura: o Revit simplifica para painéis triangulados — não é uma cobertura paramétrica contínua.
- A espessura segue a normal da superfície — em superfícies muito curvas pode haver imprecisão nas bordas.

---

## 8. Criar Pisos a partir de Faces

### 8.1 Piso por Face

1. Aba **Massa e Terreno > Modelo por Face > Piso**.
2. Clique nas faces da massa que representam a laje.
3. Clique em **Criar Piso**.

Para lajes estruturais: aba **Estrutura > Modelo por Face > Piso Estrutural**.

> Famílias de sistema (paredes, pisos, telhados) aceitam "por face". Famílias carregáveis (pilares, vigas) não usam este método — são posicionadas por pontos de referência.

---

## 9. Atualizar elementos após mudança no Rhino

Quando o modelo Rhino for modificado e um novo SAT for exportado:

1. Se usou **Vincular CAD:** `Gerenciar > Gerenciar Vínculos` → selecione o vínculo SAT → **Recarregar**.
2. Após recarregar o vínculo, selecione os elementos "por face" e use **Atualizar para Face** nas propriedades para sincronizar com a geometria nova.

> ⚠️ Faces renumeradas ou reposicionadas após reexportação podem perder o vínculo com os elementos. Confirme sempre visualmente após o recarregamento.

---

## 10. Forma Direta (Direct Shape) — uso alternativo

Para geometria que não será convertida em elementos BIM (contexto urbano, referência visual, maquete eletrônica), o SAT pode ser importado diretamente no projeto sem passar por Família de Massa:

1. `Inserir > Importar CAD` → SAT → a geometria entra como **Forma Direta**.
2. Selecione → em **Propriedades**, mude a **Categoria** para `Modelos Genéricos`, `Paredes`, `Telhados` etc.
3. A Forma Direta aparece nas vistas 3D e pode cortar em plantas conforme a categoria atribuída.

> A Forma Direta **não** suporta as ferramentas "Modelo por Face" — é apenas para referência ou visualização.

---

## 11. Configurações de visibilidade e corte em plantas

- **Visibilidade:** `Vista > Visibilidade/Gráficos (VG)` → aba **Categorias de Modelos** (para elementos criados por face) ou **Categorias Importadas** (para SAT/DWG importados diretamente).
- **Nível de detalhe:** em **Grosseiro**, geometria complexa pode simplificar — use **Médio** ou **Fino**.
- **Exibição de massas:** massas ficam ocultas por padrão. Ative em `Massa e Terreno > Exibir Massas por Vista e Tipo` quando precisar trabalhar com elas.

---

## 12. Fluxo resumido

```
Rhino 8 (inglês)                    Revit 2026 (português)
────────────────                    ──────────────────────
Organizar por layer/grupo      →    Exportar SAT por categoria
File > Properties > Units (mm) →    Unidades explícitas na importação
File > Export Selected > SAT   →    Novo > Família > Massa.rft
                                     Inserir > Importar CAD (SAT)
                                     Salvar .rfa → Carregar no projeto
                                    ↓
                              Massa e Terreno > Exibir Massas
                              Massa e Terreno > Modelo por Face:
                              - Parede (faces planas/regladas)
                              - Telhado (coberturas irregulares)
                              - Piso (lajes irregulares)
                              - Sistema Parede Cortina (dupla curvatura)
```

---

## 13. Boas práticas

- Exporte por **categoria** em SATs separados (paredes, coberturas) — facilita a seleção de faces no Revit.
- Mantenha o modelo Rhino na mesma unidade do arquivo do Revit (m -> m ou mm -> mm por exemplo).
- Nomeie os layers no Rhino com nomes descritivos — chegam no Revit e ajudam a triagem.
- Use **Vincular CAD** (não Importar) para SATs de geometria que ainda vai evoluir — permite recarregar.
- Para Parede por Face funcionar em superfícies curvas, prefira geometria com **curvatura simples** (cilíndrica) no Rhino — dupla curvatura exige Sistema de Parede Cortina.
- Evite sólidos com faces degeneradas, arestas soltas ou superfícies não fechadas — o Revit pode rejeitar na criação de elementos por face.
- Verifique sempre a direção da normal das paredes criadas por face (exterior/interior) em `Propriedades > Orientação`.
