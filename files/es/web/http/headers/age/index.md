---
title: Age
slug: Web/HTTP/Headers/Age
tags:
  - Encabezado HTTP Age
translation_of: Web/HTTP/Headers/Age
---
El encabezado **`Age`** contiene el tiempo medido en segundos que el objeto ha estado en la memoria caché de servidor proxy.El encabezado **`Age`** suele estar seteado en 0 (cero). Si **`Age : 0`** probablemente en ese instante se acaba de obtener la respuesta del servidor de origen, de lo contrario, por lo general esto se calcula como la diferencia entre la fecha actual del proxy y la fecha dada por el parámetro en cabecera "Date" ({{HTTPHeader("Date")}} ) incluído en la respuesta HTTP.

| Tipo de Cabecera                                                              | {{Glossary("Response header")}} |
| ----------------------------------------------------------------------------- | ---------------------------------------- |
| Nombre de Cabecera Prohibido {{Glossary("Forbidden header name")}} | no                                       |

## Sintaxis

```
Age: <segundos>
```

## Directivas

- \<segundos>
  - : Número entero no negativo, que representa el tiempo en segundos que el objeto ha almacenado en la caché del proxy.

## Ejemplos

```
Age: 24
```

## Especificaciones

| Specification                            | Title                                           |
| ---------------------------------------- | ----------------------------------------------- |
| {{RFC("7234", "Age", "5.1")}} | Hypertext Transfer Protocol (HTTP/1.1): Caching |

## Compatibilidad de Navegadores

{{Compat("http.headers.Age")}}

## Ver también

- {{HTTPHeader("Cache-Control")}}
- {{HTTPHeader("Expires")}}
