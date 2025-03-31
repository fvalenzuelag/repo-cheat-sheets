# Guía Rápida de Markdown

## Texto Básico

### Énfasis
- *Cursiva* = `*Cursiva*` o `_Cursiva_`
- **Negrita** = `**Negrita**` o `__Negrita__`
- ***Negrita y Cursiva*** = `***Negrita y Cursiva***`
- ~~Tachado~~ = `~~Tachado~~`

### Encabezados
```
# Encabezado 1
## Encabezado 2
### Encabezado 3
#### Encabezado 4
##### Encabezado 5
###### Encabezado 6
```

### Listas

#### Lista sin ordenar
```
- Elemento 1
- Elemento 2
  - Subelemento 2.1
  - Subelemento 2.2
```

#### Lista ordenada
```
1. Primer elemento
2. Segundo elemento
   1. Subelemento 2.1
   2. Subelemento 2.2
```

### Enlaces
- Enlace básico: `[Texto del enlace](URL)`
- Enlace con título: `[Texto del enlace](URL "Título")`
- URL directa: `<https://www.ejemplo.com>`

### Imágenes
- Imagen básica: `![Texto alternativo](URL-imagen)`
- Imagen con título: `![Texto alternativo](URL-imagen "Título de la imagen")`

## Elementos Avanzados

### Bloques de Código
#### Código en línea
`código` = `` `código` ``

#### Bloque de código
````
```lenguaje
código aquí
```
````

### Citas
```
> Esto es una cita
> Continúa aquí
>> Cita anidada
```

### Tablas
```
| Encabezado 1 | Encabezado 2 |
|--------------|--------------|
| Celda 1      | Celda 2      |
| Celda 3      | Celda 4      |
```

### Línea Horizontal
Cualquiera de estos:
```
---
***
___
```

### Escape de Caracteres
Usa `\` antes de caracteres especiales:
```
\* Esto no será cursiva \*
\` Esto no será código \`
```

### Listas de Tareas
```
- [x] Tarea completada
- [ ] Tarea pendiente
```

## Consejos Adicionales

1. Deja una línea en blanco antes y después de los encabezados
2. Usa un espacio después de los marcadores de lista (- o números)
3. Para anidar elementos, usa 2 espacios de indentación
4. Mantén consistencia en el uso de símbolos (*_ para énfasis)
5. Para tablas más legibles, alinea las barras verticales
