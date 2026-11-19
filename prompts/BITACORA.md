# Bitacora de tecnicas avanzadas
 
Laboratorio 07: Tecnicas Avanzadas de Prompting.
Herramienta de IA usada: (escribe aqui cual usaste)
 
## Ejercicio 2: Zero-shot, one-shot y few-shot 

| Tipo | Aciertos (de 5) | Formato de la respuesta | Todas con el mismo formato (Si/No) |
|------|-----------------|-------------------------|------------------------------------|
| Zero-shot | 5 | Tabla con colores, resumen de resultados y conclusión (formato libre, no pedido) | No |
| One-shot | 5 | Lista numerada con "comentario" → etiqueta en negrita (imita el ejemplo con variaciones) | Si |
| Few-shot | 5 | Una línea por comentario con el formato exacto "texto" -> etiqueta, sin texto extra | Si |
 
## Ejercicio 3: Chain of Thought 

| Pedido | Respuesta de la IA | Muestra los pasos (Si/No) | Correcta (Si/No) |
|--------|--------------------|---------------------------|------------------|
| Directo | 318.60 | No | Si |
| Paso a paso | 318.60 | Si | Si |
 
## Ejercicio 4: Role prompting 

| Version | Vocabulario (sencillo/tecnico) | Usa ejemplos o codigo | A quien le sirve mas |
|---------|-------------------------------|-----------------------|----------------------|
| A. Sin rol | Sencillo (caja con etiqueta, tipos basicos) | Si, codigo en Python y tabla de tipos de datos | Principiantes, explicacion general |
| B. Rol docente | Sencillo (parte desde cero, comparaciones de la vida diaria) | Si, caja con objetos, esquema visual, codigo en Python y tabla de tipos | Estudiantes que nunca han programado |
| C. Rol senior | Tecnico (memoria, tipo de dato, declaracion, identificador) | Si, codigo en Java con caso de un sistema de ventas | Companeros desarrolladores que ya programan |
 
## Ejercicio 5: Descomposicion 

## Ejercicio 5: Descomposicion

**Pedido de una sola vez:** "Crea un sistema de inventario para una tienda."
La IA entrego una aplicacion web completa (INVENTARIO PRO) con productos, stock, ventas, reportes CSV y copias de seguridad. Tomo decisiones sin consultarme (web en lugar de Java, un solo usuario, datos guardados en el navegador).

**Pedido por pasos (todos en el mismo chat):**

- **Paso 1:** La IA listo 5 requisitos (gestion de productos, control de entradas y salidas, control de stock, gestion de ventas y reportes y consultas), cada uno con sub-puntos, y recomendo Java con Swing, MySQL o SQLite y JDBC.
- **Paso 2:** La IA diseno 8 clases con sus atributos y tipos de dato (Producto, Inventario, Movimiento, Venta, DetalleVenta, Reporte, Usuario y ConexionBD). Le pedi que no usara ArrayList para mantener el diseno mas sencillo, por eso Inventario y Venta solo guardan un contador y no la lista de sus productos o detalles. La IA aviso que para manejar muchos registros haria falta una base de datos.
- **Paso 3:** La IA escribio la clase Producto en Java con 7 atributos privados (usando cantidadStock, como en el diseno del paso 2), un constructor y los getters y setters. Es coherente con el diseno e incluyo un ejemplo de como crear un producto.
- **Paso 4:** La IA propuso 3 mejoras para Producto: validar precios y cantidades, validar que el codigo no este vacio y agregar los metodos aumentarStock y disminuirStock. Note que la mejora 1 solo muestra el ejemplo de setPrecio (el constructor seguiria sin validar) y que aumentarStock ignora cantidades invalidas mientras disminuirStock lanza un error.

**Comparacion:** El pedido de una sola vez me dio algo grande pero sin control, y no pude revisar cada parte. El pedido por pasos me dio requisitos, un diseno de clases y el codigo de Producto en Java, coherentes entre si (por ejemplo, el atributo cantidadStock paso igual del diseno al codigo). Ademas, al revisar cada respuesta pude corregir el diseno con mis propias indicaciones y detectar detalles que mejorar.
 
## Ejercicio 6: Prompt estructurado y autocritica 

## Ejercicio 6: Prompt estructurado y autocritica

**Prompt basico:** "Dame casos de prueba para un login."
La IA entrego 34 casos en 6 categorias, con columnas distintas en cada tabla, y agrego una plantilla y una recomendacion que no pedi. No considero que la cuenta se bloquea despues de 3 intentos fallidos y varios resultados esperados son generales.

**Prompt estructurado:** la IA entrego una sola tabla con 6 casos y las 4 columnas pedidas. Uso el contexto: 4 de los 6 casos prueban el bloqueo tras 3 intentos fallidos. Explico sus suposiciones. Le faltaron casos limite como campos vacios, correo sin @ y contrasena con espacios.

**Autocritica:** la IA reconocio lo que faltaba, agrego 6 casos nuevos (CP-07 a CP-12: ambos campos vacios, correo vacio, contrasena vacia, correo sin @, contrasena con espacios y correo con espacios) e indico cuales eran en una tabla aparte. Total: 12 casos.

### Evaluacion de la tabla final

| Que revisar | Cumple (Si / No) |
|-------------|------------------|
| Tiene las 4 columnas pedidas? | Si |
| Incluye el bloqueo despues de 3 intentos? | Si |
| Incluye casos con campos vacios? | Si |
| Indica que casos agrego en la autocritica? | Si |
| Hay algun caso repetido o que no tenga sentido? | No |

**Observaciones del revisor:** no hay casos repetidos, pero CP-11 y CP-12 tienen resultados esperados poco verificables ("aplica la regla definida"), porque no dicen que debe pasar exactamente. Tambien quedo una linea suelta ("6 casos nuevos") antes de la tabla de casos agregados. La IA propuso por su cuenta que los errores de validacion no cuenten como intentos fallidos, pero eso debe confirmarse con los requisitos del sistema.

### Prompt estructurado y mensaje de autocritica

```text
<rol>Actua como analista de pruebas de software.</rol>
<contexto>Login web con correo y contrasena. La cuenta se bloquea
despues de 3 intentos fallidos.</contexto>
<tarea>Piensa paso a paso que puede fallar y escribe 6 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada,
resultado esperado.</formato>

Revisa tu tabla: faltan casos limite como campos vacios, correo sin @
o contrasena con espacios? Agrega los que falten e indica cuales agregaste.
```
