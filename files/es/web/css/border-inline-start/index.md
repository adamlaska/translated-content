---
title: border-inline-start
slug: Web/CSS/border-inline-start
translation_of: Web/CSS/border-inline-start
---
{{CSSRef}}{{SeeCompatTable}}

La propiedad de [CSS](/es/docs/Web/CSS) **`border-inline-start`** es una [propiedad abreviada](/es/docs/Web/CSS/Shorthand_properties) para establecer los valores de la propiedad inicial del borde individual en línea en un solo lugar en la hoja de estilos.

{{EmbedInteractiveExample("pages/css/border-inline-start.html")}}

## Sintaxis

```css
border-inline-start: 1px;
border-inline-start: 2px dotted;
border-inline-start: medium dashed green;
```

`border-inline-start` es especificado con uno o más de {{cssxref("border-inline-start-width")}}, {{cssxref("border-inline-start-style")}}, and {{cssxref("border-inline-start-color")}}. El borde físico al que se mapea depende del modo de escritura, la direccionalidad y la orientación del texto del elemento. Esto corresponde a las propiedades {{cssxref("border-top")}}, {{cssxref("border-right")}}, {{cssxref("border-bottom")}}, o {{cssxref("border-left")}} dependiendo de los valores definidos por {{cssxref("writing-mode")}}, {{cssxref("direction")}}, y {{cssxref("text-orientation")}}.

Propiedades relacionadas son {{cssxref("border-block-start")}}, {{cssxref("border-block-end")}}, and {{cssxref("border-inline-end")}}, que definen los otros bordes del elemento.

{{cssinfo}}

### Valores

El `border-inline-start` es especificado con uno o más de los sigueintes valores, en cualquier orden:

- `<'border-width'>`
  - : El ancho del borde. Mira {{cssxref("border-width")}}.
- `<'border-style'>`
  - : La línea de estilo del borde. Mira {{cssxref("border-style")}}.
- `<'color'>`
  - : El color del borde. Mira {{cssxref("color")}}.

### Sintaxis formal

{{csssyntax}}

## Ejemplo

### Contenido HTML

```html
<div>
  <p class="exampleText">Example text</p>
</div>
```

### Contenido CSS

```css
div {
  background-color: yellow;
  width: 120px;
  height: 120px;
}

.exampleText {
  writing-mode: vertical-rl;
  border-inline-start: 5px dashed blue;
}
```

{{EmbedLiveSample("Ejemplo", 140, 140)}}

## Especificación

| Especificación                                                                                                               | Estado                                           | Comentario          |
| ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------------- |
| {{SpecName("CSS Logical Properties", "#propdef-border-inline-start", "border-inline-start")}} | {{Spec2("CSS Logical Properties")}} | Definición inicial. |

## Compatibilidad en navegadores

{{Compat("css.properties.border-inline-start")}}

## Mira también

- Esta propiedad se asigna a una de las propiedades del borde físico: {{cssxref("border-top")}}, {{cssxref("border-right")}}, {{cssxref("border-bottom")}}, o {{cssxref("border-left")}}.
- {{cssxref("writing-mode")}}, {{cssxref("direction")}}, {{cssxref("text-orientation")}}
