# Tarea: Mi prompt avanzado

## Tarea elegida

Generar casos de prueba para un **formulario de registro de usuarios**, actuando como analista de pruebas de software.

Reglas del formulario:

- Campos: correo, contrasena y confirmar contrasena.
- La contrasena debe tener minimo 8 caracteres, una mayuscula y un numero.
- El correo no puede estar ya registrado.

Elegi esta tarea porque es parecida a un trabajo real de QA y porque sus reglas son claras, asi que puedo comprobar si la respuesta de la IA es correcta.

## Version 1: prompt basico

```text
Dame casos de prueba para un formulario de registro de usuarios.
```

**Tecnica agregada:** ninguna. Es un prompt basico de una sola instruccion.

**Por que:** es el punto de partida para comparar con las versiones siguientes.

**Resultado:** la IA entrego 39 casos en 5 categorias, con tablas de 3 columnas (sin datos de entrada) y extras que no pedi (un caso detallado, un resumen de cobertura y una oferta de matriz en Excel). No conocia las reglas de mi formulario, por eso sus resultados esperados son vagos ("segun las reglas del negocio") e invento un campo Nombre que no existe.

**Que le falto:** un rol, el contexto del formulario (campos y reglas de la contrasena), una cantidad definida de casos y un formato de respuesta fijo.

## Version 2: rol y prompt estructurado

```text
<rol>Actua como analista de pruebas de software que prueba formularios web.</rol>
<contexto>Formulario de registro con correo, contrasena y confirmar contrasena.
La contrasena debe tener minimo 8 caracteres, una mayuscula y un numero.
El correo no puede estar ya registrado.</contexto>
<tarea>Escribe 8 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada,
resultado esperado.</formato>
```

**Tecnicas agregadas:** role prompting (analista de pruebas de software que prueba formularios web) y prompt estructurado (etiquetas rol, contexto, tarea y formato).

**Por que:** en la version 1 la IA no conocia mis reglas, invento un campo y entrego 39 casos con formato libre. El rol le da el punto de vista de un tester, el contexto le da las reglas reales del formulario y las etiquetas separan cada parte para que no se mezclen.

**Que mejoro:** entrego exactamente 8 casos en una sola tabla con las 4 columnas pedidas. Cada regla (8 caracteres, mayuscula, numero, correo ya registrado y confirmacion) tiene su caso, y dejo de inventar el campo Nombre. Tambien aclaro sus suposiciones al final.

**Que sigue faltando:** casos limite (una contrasena de exactamente 8 caracteres, correo o contrasena con espacios), casos con un solo campo vacio, el razonamiento detras de la eleccion de casos y una revision final. Ademas, el formato de los datos de entrada no estaba fijado con un ejemplo.

## Version 3: prompt final

```text
<rol>Actua como analista de pruebas de software que prueba formularios web.</rol>
<contexto>Formulario de registro con correo, contrasena y confirmar contrasena.
La contrasena debe tener minimo 8 caracteres, una mayuscula y un numero.
El correo no puede estar ya registrado.</contexto>
<ejemplo>
ID: CP-01 | Escenario: Registro exitoso | Datos de entrada: correo nuevo, contrasena Segura123, confirmacion Segura123 | Resultado esperado: cuenta creada y mensaje de confirmacion
</ejemplo>
<tarea>Piensa paso a paso que reglas tiene el formulario y que puede fallar en
cada una. Luego escribe 8 casos de prueba que cubran casos validos, invalidos
y limite. Al terminar, revisa tu tabla, agrega los casos que falten e indica
cuales agregaste.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada,
resultado esperado. Sigue el formato del ejemplo.</formato>
```

**Tecnicas agregadas:** un ejemplo del formato esperado (one-shot), chain of thought (pensar paso a paso las reglas y lo que puede fallar) y autocritica (revisar la tabla y agregar lo que falte).

**Por que:** en la version 2 la tabla tenia buen formato, pero no mostraba por que eligio esos casos, no incluia casos limite (como una contrasena de exactamente 8 caracteres), no se revisaba y los datos de entrada venian con saltos de linea que podian variar. El ejemplo fija el formato, el paso a paso obliga a analizar cada regla y la autocritica busca lo que quedo sin cubrir.

**Que mejoro:** antes de la tabla la IA muestra un analisis de reglas y posibles fallos. Los datos de entrada siguen el formato del ejemplo (una linea separada por comas). La autocritica detecto que faltaba el limite de 8 caracteres y agrego CP-09 (exactamente 8 caracteres, se acepta) y CP-10 (8 caracteres sin mayuscula ni numero, se rechaza), indicando cuales eran. Total: 10 casos.

**Que sigue faltando:** casos con espacios en el correo o la contrasena y casos con un solo campo vacio. Ademas, la clasificacion final que dio la IA (2 validos, 7 invalidos y 1 limite) es inconsistente, porque CP-09 y CP-10 son casos limite.

## Tecnicas usadas en el prompt final

| Parte del prompt | Tecnica |
|------------------|---------|
| `<rol>Actua como analista de pruebas de software que prueba formularios web.</rol>` | Role prompting |
| Etiquetas `<rol>`, `<contexto>`, `<tarea>` y `<formato>` | Prompt estructurado |
| `<ejemplo>` con un caso de prueba (CP-01) | One-shot (un ejemplo) |
| "Piensa paso a paso que reglas tiene el formulario y que puede fallar en cada una" | Chain of thought |
| "Al terminar, revisa tu tabla, agrega los casos que falten e indica cuales agregaste" | Autocritica |

## Evaluacion del resultado

| Criterio | Cumple (Si / No) |
|----------|------------------|
| Tiene las 4 columnas pedidas (ID, escenario, datos de entrada, resultado esperado)? | Si |
| Muestra el razonamiento paso a paso antes de los casos? | Si |
| Sigue el formato del ejemplo (datos de entrada en una sola linea)? | Si |
| Incluye el limite exacto de 8 caracteres de la contrasena? | Si |
| Cubre el correo ya registrado y la confirmacion que no coincide? | Si |
| Indica que casos agrego en la autocritica? | Si |
| Incluye casos con espacios o con un solo campo vacio? | No |
| La clasificacion final de los casos es coherente? | No |

## Por que elegi estas tecnicas

Elegi un rol especifico (analista de pruebas que prueba formularios web) para que la IA respondiera con el enfoque de un tester y no de forma generica, y un prompt estructurado para que las reglas del formulario no se mezclaran con la tarea ni con el formato. Use chain of thought porque probar un formulario exige pensar primero que reglas tiene y que puede fallar en cada una, y asi pude comprobar el razonamiento antes de la tabla. Agregue un ejemplo para fijar el formato de cada fila y la autocritica porque la version 2 no incluia casos limite y la IA podia detectarlos al revisarse. 