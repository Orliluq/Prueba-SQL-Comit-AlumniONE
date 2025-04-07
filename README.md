# Prueba Técnica de SQL – Duración: 1 Semana 🗂️

## Tabla de Contenidos 📚

- [Prueba Técnica de SQL – Duración: 1 Semana 🗂️](#prueba-técnica-de-sql--duración-1-semana-️)
  - [Tabla de Contenidos 📚](#tabla-de-contenidos-)
  - [Instrucciones Generales 📝](#instrucciones-generales-)
  - [Ejercicios 🧩](#ejercicios-)
    - [Ejercicio 1: Productos con Precio Superior al Promedio 📊](#ejercicio-1-productos-con-precio-superior-al-promedio-)
    - [Ejercicio 2: Eliminación de Duplicados con UNION 🔄](#ejercicio-2-eliminación-de-duplicados-con-union-)
    - [Ejercicio 3: Conteo de Pedidos en 2024 📅](#ejercicio-3-conteo-de-pedidos-en-2024-)
    - [Ejercicio 4: Clasificación de Clientes por Total de Pedidos 🏷️](#ejercicio-4-clasificación-de-clientes-por-total-de-pedidos-️)
    - [Ejercicio 5: Total de Ventas por Categoría 📦](#ejercicio-5-total-de-ventas-por-categoría-)
    - [Ejercicio 6: Primera Venta en el Mes de Creación 🗓️](#ejercicio-6-primera-venta-en-el-mes-de-creación-️)
    - [Ejercicio 7: Combinación de Ventas de 2023 y 2024 🔀](#ejercicio-7-combinación-de-ventas-de-2023-y-2024-)
    - [Ejercicio 8: Empleados en Múltiples Proyectos 👥](#ejercicio-8-empleados-en-múltiples-proyectos-)
  - [Criterios de Evaluación ✅](#criterios-de-evaluación-)
  - [Roles y Aplicación en el Sector 💼](#roles-y-aplicación-en-el-sector-)
  - [Entrega Final 📤](#entrega-final-)

## Instrucciones Generales 📝

**Tiempo de Ejecución:** 1 semana.

**Entregables:**

- Un archivo `.sql` que contenga los scripts de cada ejercicio, debidamente comentados.
- Un documento en formato PDF o Markdown con explicaciones y justificaciones de las soluciones implementadas (opcional pero recomendado).

**Ambiente:** Puedes utilizar cualquier motor de base de datos relacional (MySQL, PostgreSQL, SQL Server, etc.). Indica el motor utilizado en los comentarios del archivo de scripts.

---

## Ejercicios 🧩

### Ejercicio 1: Productos con Precio Superior al Promedio 📊

**Enunciado:**  
Para la tabla `productos` (campos: `id_producto`, `nombre`, `precio`), escribe una consulta que muestre los nombres de los productos cuyo precio sea superior al precio promedio de todos los productos.

**Ejemplo esperado:**

```sql
SELECT nombre
FROM productos
WHERE precio > (SELECT AVG(precio) FROM productos);
```

---

### Ejercicio 2: Eliminación de Duplicados con UNION 🔄

**Enunciado:**  
Considere las siguientes opciones de consultas que combinan datos de las tablas `clientes_1` y `clientes_2` junto con la tabla `compras`:

- **Opción A:**

  ```sql
  SELECT nombre, valor
  FROM clientes_1
  JOIN compras ON clientes_1.id_cliente = compras.id_cliente
  UNION
  SELECT nombre, valor
  FROM clientes_2
  JOIN compras ON clientes_2.id_cliente = compras.id_cliente;
  ```

- **Opción B:**

  ```sql
  SELECT nombre, valor
  FROM clientes_1
  JOIN compras ON clientes_1.id_cliente = compras.id_cliente
  UNION ALL
  SELECT nombre, valor
  FROM clientes_2
  JOIN compras ON clientes_2.id_cliente = compras.id_cliente;
  ```

- **Opción C:**

  ```sql
  SELECT DISTINCT nombre, valor
  FROM clientes_1
  JOIN compras ON clientes_1.id_cliente = compras.id_cliente
  UNION ALL
  SELECT DISTINCT nombre, valor
  FROM clientes_2
  JOIN compras ON clientes_2.id_cliente = compras.id_cliente;
  ```

**Pregunta:**  
Indica cuál de las opciones garantiza que el resultado final no contenga duplicados y justifica tu respuesta.

---

### Ejercicio 3: Conteo de Pedidos en 2024 📅

**Enunciado:**  
Para la tabla `pedidos` (campos: `id_pedido`, `id_cliente`, `fecha_pedido`, `total`), escribe una consulta que muestre el `id_cliente` y el número de pedidos realizados por cada cliente en el año 2024. Incluye únicamente aquellos clientes que hayan realizado más de 5 pedidos.

**Ejemplo esperado:**

```sql
SELECT id_cliente, COUNT(*) AS num_pedidos
FROM pedidos
WHERE YEAR(fecha_pedido) = 2024
GROUP BY id_cliente
HAVING COUNT(*) > 5;
```

---

### Ejercicio 4: Clasificación de Clientes por Total de Pedidos 🏷️

**Enunciado:**  
Dadas las tablas `Clientes` y `Pedidos`, escribe una consulta que muestre el nombre del cliente y, en una columna adicional, indique:

- "Alto" si el total de sus pedidos es mayor a $5000.
- "Medio" si el total está entre $3000 y $5000.
- "Bajo" si es menor a $3000.

**Ejemplo esperado:**

```sql
SELECT c.nombre,
       CASE
         WHEN SUM(p.total) > 5000 THEN 'Alto'
         WHEN SUM(p.total) BETWEEN 3000 AND 5000 THEN 'Medio'
         ELSE 'Bajo'
       END AS clasificacion
FROM Clientes c
JOIN Pedidos p ON c.id_cliente = p.id_cliente
GROUP BY c.nombre;
```

---

### Ejercicio 5: Total de Ventas por Categoría 📦

**Enunciado:**  
Para las tablas `ventas` y `productos`, escribe una consulta que muestre el total de la cantidad de productos vendidos por cada categoría.

**Ejemplo esperado:**

```sql
SELECT p.categoria, SUM(v.cantidad) AS total_vendido
FROM ventas v
JOIN productos p ON v.id_producto = p.id_producto
GROUP BY p.categoria;
```

---

### Ejercicio 6: Primera Venta en el Mes de Creación 🗓️

**Enunciado:**  
Dadas las tablas `Ventas` y `Productos`, escribe una consulta para mostrar el `producto_id` de aquellos productos que fueron vendidos por primera vez en el mismo mes y año en el que fueron creados.

**Ejemplo sugerido:**

```sql
SELECT v.producto_id
FROM Ventas v
JOIN Productos p ON v.producto_id = p.producto_id
WHERE YEAR(v.fecha_venta) = YEAR(p.fecha_creacion)
  AND MONTH(v.fecha_venta) = MONTH(p.fecha_creacion)
  AND v.fecha_venta = (
      SELECT MIN(fecha_venta)
      FROM Ventas
      WHERE producto_id = v.producto_id
  );
```

---

### Ejercicio 7: Combinación de Ventas de 2023 y 2024 🔀

**Enunciado:**  
Dadas las tablas `Venta_2023` y `Ventas_2024`, escribe una consulta que combine las ventas de ambos años y añada una columna adicional llamada `año`.

**Ejemplo esperado:**

```sql
SELECT venta_id, producto_id, cantidad, precio_unitario, 2023 AS año
FROM Venta_2023
UNION ALL
SELECT venta_id, producto_id, cantidad, precio_unitario, 2024 AS año
FROM Ventas_2024;
```

---

### Ejercicio 8: Empleados en Múltiples Proyectos 👥

**Enunciado:**  
Dadas las tablas `Empleados` y `Proyectos`, escribe una consulta que muestre el nombre de los empleados que trabajan en más de un proyecto distinto.

**Ejemplo esperado:**

```sql
SELECT e.nombre
FROM Empleados e
JOIN Proyectos p ON e.empleado_id = p.empleado_id
GROUP BY e.empleado_id, e.nombre
HAVING COUNT(DISTINCT p.proyecto_id) > 1;
```

---

## Criterios de Evaluación ✅

- **Corrección y Funcionalidad:** Cada consulta debe cumplir con lo solicitado y funcionar correctamente en el motor de base de datos elegido.
- **Claridad y Legibilidad:** Se valorará el uso de comentarios y la claridad en la estructura de las consultas.
- **Buenas Prácticas:** Se evaluará la eficiencia de las consultas y el uso adecuado de funciones, operadores y subconsultas.

---

## Roles y Aplicación en el Sector 💼

Esta prueba técnica está orientada a evaluar competencias fundamentales en SQL para roles como:

- Desarrollador de Bases de Datos / DBA Junior
- Analista de Datos Junior
- Desarrollador Backend con conocimientos en SQL
- Especialista en Business Intelligence (nivel inicial)

---

## Entrega Final 📤

Los candidatos deberán enviar su solución completa (archivos `.sql` y documentación opcional) antes de la finalización del período de 1 semana.

---

## ✅ Análisis de lo que se espera en cada ejercicio

### Ejercicio 1: Productos con Precio Superior al Promedio
Se evalúa:

- Uso de subconsultas en cláusulas `WHERE`.

- Capacidad de comparación con valores agregados.

### Ejercicio 2: Eliminación de Duplicados con UNION
Se evalúa:

- Comprensión de la diferencia entre `UNION`, `UNION ALL` y `DISTINCT`.

- Saber cómo evitar duplicados correctamente.

### Ejercicio 3: Conteo de Pedidos en 2024
Se evalúa:

- Filtrado por año `(YEAR(fecha_pedido))`.

- Agrupamiento `(GROUP BY)` y uso de `HAVING` para filtrar agregados.

### Ejercicio 4: Clasificación de Clientes por Total de Pedidos
Se evalúa:

- Uso del `CASE` para clasificaciones condicionales.

- Agregación `(SUM)` y lógica de negocio para segmentación de clientes.

### Ejercicio 5: Total de Ventas por Categoría
Se evalúa:

- Habilidad para relacionar tablas con `JOIN`.

- Agrupamiento por campo categórico y sumar cantidades `(SUM)`.

### Ejercicio 6: Primera Venta en el Mes de Creación
Se evalúa:

- Comparación de fechas `(YEAR, MONTH)`.

- Uso de subconsultas correlacionadas para detectar primeros eventos.

### Ejercicio 7: Combinación de Ventas de 2023 y 2024
Se evalúa:

- Correcto uso de `UNION ALL` y creación de columnas adicionales literales.

- Unión de datos similares de distintos años.

### Ejercicio 8: Empleados en Múltiples Proyectos
Se evalúa:

- Agrupamiento y conteo de elementos distintos `(COUNT(DISTINCT ...))`.

- Saber identificar relaciones uno-a-muchos.

## 🎯 Conclusión
La prueba busca evaluar tanto conocimiento técnico de SQL como comprensión analítica de negocio, algo fundamental para roles en Data Analysis, Data Engineering o Backend Development. No solo se trata de escribir consultas correctas, sino también de entender el propósito de cada una y cómo extraer valor de los datos.

## ¡Buena suerte y éxitos en el proceso! 🚀
