# Especificação — Borda Dupla Roxa em SVG para Todo o Site

## Objetivo

Criar uma **borda dupla roxa em formato SVG** que contorne visualmente toda a extensão da página do site.

A borda deve acompanhar **automaticamente a largura e principalmente a altura total do conteúdo da página**. Se a página crescer de 1.000 px para 3.000 px, 5.000 px ou mais de altura, a borda deverá continuar envolvendo todo o conteúdo, sem ser necessário alterar manualmente o SVG.

A borda deve ser apenas decorativa e não pode interferir na interação do usuário com o site.

---

## Requisitos visuais

- Criar uma borda dupla contínua ao redor de todo o site.
- Cor predominante: roxo.
- Estilo elegante, delicado e compatível com uma identidade visual floral/aquarelada.
- As duas linhas devem ser paralelas.
- A linha externa deve ficar próxima às extremidades da página.
- A linha interna deve possuir um pequeno afastamento da linha externa.
- O centro do SVG deve ser totalmente transparente.
- Não adicionar preenchimento de fundo.
- Não adicionar flores, folhas, borboletas ou outros elementos neste SVG.
- O SVG deve conter somente a borda.
- A espessura deve ser discreta para não competir com o conteúdo.
- Os cantos devem ser limpos e contínuos.
- Evitar bordas excessivamente grossas.

### Sugestão de cores

Usar uma tonalidade semelhante a:

- Roxo principal: `#7B4AA8`
- Roxo secundário mais claro: `#A77BC5`

As cores podem ser ajustadas caso seja necessário harmonizar melhor com o restante do site.

---

## Requisito mais importante: altura dinâmica

**Não criar o SVG com uma altura fixa**, como:

```text
height="3500"
```

ou:

```text
viewBox="0 0 1920 3500"
```

com a intenção de representar uma página específica.

A implementação deve permitir que a borda acompanhe o tamanho real da página.

Por exemplo:

```text
Página com 1200px de altura
        ↓
SVG = 1200px de altura

Página com 3500px de altura
        ↓
SVG = 3500px de altura

Página com 6000px de altura
        ↓
SVG = 6000px de altura
```

A altura deve ser determinada pelo elemento/container que envolve o conteúdo da página.

---

## Arquitetura recomendada

Preferencialmente, utilizar um container principal que envolva **todo o conteúdo do site**.

Exemplo conceitual:

```html
<div class="site-wrapper">

    <svg class="site-border" ...>
        ...
    </svg>

    <header>
        ...
    </header>

    <main>
        ...
    </main>

    <footer>
        ...
    </footer>

</div>
```

O `.site-wrapper` deve possuir altura suficiente para abranger todo o conteúdo.

O SVG deve ocupar exatamente a área desse container:

```css
.site-wrapper {
    position: relative;
    width: 100%;
    min-height: 100vh;
}

.site-border {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
}
```

O objetivo é que:

```text
width: 100%
height: 100%
```

façam o SVG acompanhar automaticamente o tamanho do `.site-wrapper`.

---

## Comportamento em páginas longas

A solução deve funcionar mesmo quando o conteúdo ultrapassar a altura da tela.

Por exemplo:

```text
┌─────────────────────────────────────────┐
│ ╔═════════════════════════════════════╗ │
│ ║                                     ║ │
│ ║             HEADER                  ║ │
│ ║                                     ║ │
│ ║─────────────────────────────────────║ │
│ ║                                     ║ │
│ ║             CONTEÚDO                ║ │
│ ║                                     ║ │
│ ║             CONTEÚDO                ║ │
│ ║                                     ║ │
│ ║             CONTEÚDO                ║ │
│ ║                                     ║ │
│ ║              FOOTER                 ║ │
│ ║                                     ║ │
│ ╚═════════════════════════════════════╝ │
└─────────────────────────────────────────┘
```

A borda deve terminar somente no final real da página.

**Não utilizar `position: fixed` para a borda**, pois isso faria a borda acompanhar somente o viewport e não necessariamente toda a altura do documento.

---

## SVG

Criar um SVG simples e leve.

Exemplo de estrutura aceitável:

```svg
<svg
    class="site-border"
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 100 100"
    preserveAspectRatio="none"
    aria-hidden="true">

    <rect
        x="1"
        y="1"
        width="98"
        height="98"
        fill="none"
        stroke="#7B4AA8"
        stroke-width="0.7"/>

    <rect
        x="2.8"
        y="2.8"
        width="94.4"
        height="94.4"
        fill="none"
        stroke="#A77BC5"
        stroke-width="0.35"/>

</svg>
```

O exemplo acima é apenas uma referência.

A implementação final deve ser ajustada para evitar distorções visuais e manter uma aparência elegante.

---

## Atenção ao `preserveAspectRatio`

Avaliar cuidadosamente o uso de:

```xml
preserveAspectRatio="none"
```

O SVG precisa acompanhar uma página que pode ter proporções muito diferentes entre desktop e mobile.

Caso o uso de `preserveAspectRatio="none"` faça a espessura ou os cantos da borda ficarem visualmente deformados, utilizar uma abordagem CSS/SVG alternativa que mantenha a espessura visual da linha.

A prioridade é:

1. borda acompanhar toda a altura da página;
2. borda acompanhar toda a largura;
3. espessura visual consistente;
4. aparência correta em desktop e mobile.

---

## Responsividade

A solução deve funcionar em:

- Desktop
- Notebook
- Tablet
- Smartphone

Não assumir uma largura específica, como:

```text
1920px
1366px
1080px
```

A borda deve utilizar dimensões relativas.

Também não assumir uma altura fixa.

---

## Margens da borda

A borda não deve ficar exatamente colada ao limite do viewport.

Criar um pequeno espaçamento interno.

Exemplo conceitual:

```text
┌──────────────────────────────────────────┐
│                                          │
│   ╔══════════════════════════════════╗   │
│   ║                                  ║   │
│   ║            CONTEÚDO              ║   │
│   ║                                  ║   │
│   ╚══════════════════════════════════╝   │
│                                          │
└──────────────────────────────────────────┘
```

O espaçamento deve ser pequeno e elegante.

No mobile, reduzir esse espaçamento para evitar que a borda consuma espaço excessivo.

---

## Interação

O SVG deve ser puramente decorativo.

Obrigatoriamente:

```css
pointer-events: none;
```

Assim:

- links continuam funcionando;
- botões continuam funcionando;
- menus continuam funcionando;
- formulários continuam funcionando;
- a borda não captura cliques.

---

## Performance

A solução deve ser leve.

Preferir:

- SVG inline ou arquivo SVG pequeno;
- somente dois elementos `<rect>` ou elementos equivalentes;
- nenhuma imagem raster dentro do SVG;
- nenhum filtro pesado;
- nenhuma animação;
- nenhum JavaScript apenas para desenhar a borda.

**Não utilizar uma imagem PNG/JPG gigantesca para representar a borda.**

---

## Não fazer

Não implementar da seguinte forma:

```css
background-image: url("borda-3500px.png");
```

Não criar uma imagem com:

```text
3500px
5000px
6000px
```

apenas para tentar prever a altura da página.

Não utilizar:

```css
position: fixed;
height: 100vh;
```

para representar a borda completa da página.

Não criar uma borda que termine antes do footer.

Não colocar o conteúdo dentro do SVG.

---

## Entregáveis

Gerar:

1. `site-border.svg`
   - SVG da borda dupla roxa.
   - Transparente no interior.
   - Leve e escalável.

2. Exemplo de integração HTML/CSS mostrando como aplicar o SVG ao container que representa toda a página.

3. Garantir que o exemplo funcione quando o conteúdo aumentar ou diminuir de tamanho.

4. Explicar brevemente como a implementação garante que a borda acompanhe a altura total da página.

---

## Critério de aceitação

A implementação será considerada correta somente se:

- A borda aparecer nos quatro lados da página.
- Existirem duas linhas roxas.
- O centro permanecer transparente.
- A borda acompanhar a altura total do conteúdo.
- A borda acompanhar a largura disponível.
- Funcionar em páginas maiores que a altura da tela.
- Funcionar em páginas com altura variável.
- Funcionar em desktop e mobile.
- Não exigir alteração manual do SVG quando o conteúdo crescer.
- Não bloquear cliques ou interação.
- Não utilizar uma imagem raster de altura fixa.
- O SVG permanecer leve.

### Resultado esperado

O resultado final deve ser uma moldura dupla roxa, elegante e responsiva, funcionando como uma decoração permanente em torno de **toda a página**, independentemente do tamanho final do conteúdo.
