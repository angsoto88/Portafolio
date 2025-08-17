# 🧠 Subconsultas SQL en el Modelo de Jardinería

Este documento presenta una variedad de ejercicios resueltos utilizando **subconsultas SQL** aplicadas al modelo relacional ya mencionado. El objetivo es demostrar el dominio de técnicas avanzadas de consulta en bases de datos relacionales, utilizando subconsultas en diferentes contextos y operadores.

## 📘 ¿Qué contiene este archivo?

El archivo está organizado en secciones que agrupan las subconsultas según el tipo de operador o cláusula utilizada:

- **Operadores básicos de comparación** (`=`, `>`, `<`, etc.)
- **Operadores ALL y ANY**
- **Operadores IN y NOT IN**
- **Cláusulas EXISTS y NOT EXISTS**

Cada consulta está numerada y explicada mediante su código SQL y resultados visuales, permitiendo su ejecución directa en entornos compatibles con MySQL o PostgreSQL.

## 🧩 Contexto del modelo

Las consultas se basan en el modelo relacional de la base de datos **Jardinería**, que incluye las siguientes entidades:

- `cliente`: información de clientes, incluyendo su representante de ventas.
- `empleado`: datos de empleados, jefes y oficinas.
- `oficina`: ubicación y contacto de las oficinas.
- `pago`: transacciones realizadas por los clientes.
- `pedido` y `detalle_pedido`: pedidos realizados y sus productos.
- `producto` y `gama_producto`: catálogo de productos y sus gamas.

Este modelo permite realizar análisis complejos sobre ventas, pagos, jerarquías internas, y comportamiento de clientes.

## 🎯 Propósito del ejercicio

El propósito de este archivo es:

- Aplicar subconsultas en escenarios reales de negocio.
- Explorar distintas formas de filtrar, comparar y correlacionar datos.
- Preparar material técnico para documentación, evaluación y visualización en Power BI.
- Consolidar buenas prácticas en SQL para portafolios profesionales.


## Con operadores básicos de comparación
1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
SELECT nombre_cliente
FROM cliente
WHERE limite_credito = (
    SELECT MAX(limite_credito)
    FROM cliente
);
```
<p align="center">
<img src="./imagenes/subconsulta 1.png" alt="subconsulta " width="200"/>

2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
SELECT nombre
FROM producto
WHERE precio_venta = (
    SELECT MAX(precio_venta)
    FROM producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 2.png" alt="subconsulta " width="200"/>
  
3. Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla detalle_pedido)
```sql
SELECT nombre
FROM producto
WHERE codigo_producto = (
    SELECT codigo_producto
    FROM detalle_pedido
    GROUP BY codigo_producto
    ORDER BY SUM(cantidad) DESC
    LIMIT 1
);
```
<p align="center">
<img src="./imagenes/subconsulta 3.png" alt="subconsulta " width="200"/>
  
4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar INNER JOIN).
```sql
SELECT nombre_cliente
FROM cliente
WHERE limite_credito > (
    SELECT SUM(total)
    FROM pago
    WHERE cliente.codigo_cliente = pago.codigo_cliente
);
```
<p align="center">
<img src="./imagenes/subconsulta 4.png" alt="subconsulta " width="200"/>
  
5. Devuelve el producto que más unidades tiene en stock.
```sql
SELECT nombre
FROM producto
WHERE cantidad_en_stock = (
    SELECT MAX(cantidad_en_stock)
    FROM producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 5.png" alt="subconsulta " width="200"/>
  
6. Devuelve el producto que menos unidades tiene en stock.
```sql
SELECT nombre
FROM producto
WHERE cantidad_en_stock = (
    SELECT MIN(cantidad_en_stock)
    FROM producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 6.png" alt="subconsulta " width="200"/>
  
7. Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de Alberto Soria.
```sql
SELECT e.nombre, e.apellido1, e.email
FROM empleado e
WHERE codigo_jefe = (
    SELECT codigo_empleado
    FROM empleado
    WHERE nombre = 'Alberto' AND apellido1 = 'Soria'
);
```
<p align="center">
<img src="./imagenes/subconsulta 7.png" alt="subconsulta " width="300"/>
  
## Subconsultas con ALL y ANY
8. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
SELECT nombre_cliente
FROM cliente
WHERE limite_credito >= ALL (
    SELECT limite_credito
    FROM cliente
);
```
<p align="center">
<img src="./imagenes/subconsulta 8.png" alt="subconsulta " width="200"/>
  
9. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
SELECT nombre
FROM producto
WHERE precio_venta >= ALL (
    SELECT precio_venta
    FROM producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 9.png" alt="subconsulta " width="200"/>
  
10. Devuelve el producto que menos unidades tiene en stock.
```sql
SELECT nombre
FROM producto
WHERE cantidad_en_stock <= ALL (
    SELECT cantidad_en_stock
    FROM producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 10.png" alt="subconsulta " width="200"/>
  
## Subconsultas con IN y NOT IN
11. Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.
```sql
SELECT nombre, apellido1, puesto
FROM empleado
WHERE codigo_empleado NOT IN (
    SELECT codigo_empleado_rep_ventas
    FROM cliente
    WHERE codigo_empleado_rep_ventas IS NOT NULL
);
```
<p align="center">
<img src="./imagenes/subconsulta 11.png" alt="subconsulta " width="300"/>
  
12. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql
SELECT nombre_cliente
FROM cliente
WHERE codigo_cliente NOT IN (
    SELECT codigo_cliente
    FROM pago
);
```
<p align="center">
<img src="./imagenes/subconsulta 12.png" alt="subconsulta " width="200"/>
  
13. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql
SELECT nombre_cliente
FROM cliente
WHERE codigo_cliente IN (
    SELECT codigo_cliente
    FROM pago
);
```
<p align="center">
<img src="./imagenes/subconsulta 13.png" alt="subconsulta " width="200"/>
  
14. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql
SELECT nombre
FROM producto
WHERE codigo_producto NOT IN (
    SELECT codigo_producto
    FROM detalle_pedido
);
```
<p align="center">
<img src="./imagenes/subconsulta 14.png" alt="subconsulta " width="300"/>
  
15. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.
```sql
SELECT e.nombre, e.apellido1, e.puesto, o.telefono
FROM empleado e
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
WHERE e.codigo_empleado NOT IN (
    SELECT codigo_empleado_rep_ventas
    FROM cliente
);
```
<p align="center">
<img src="./imagenes/subconsulta 15.png" alt="subconsulta " width="300"/>
  
16. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.
```sql
SELECT codigo_oficina, ciudad
FROM oficina
WHERE codigo_oficina NOT IN (
    SELECT DISTINCT e.codigo_oficina
    FROM empleado e
    JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    JOIN detalle_pedido dp ON p.codigo_pedido = dp.codigo_pedido
    JOIN producto pr ON dp.codigo_producto = pr.codigo_producto
    WHERE pr.gama = 'Frutales'
);
```
<p align="center">
<img src="./imagenes/subconsulta 16.png" alt="subconsulta " width="200"/>
  
17. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
```sql
SELECT nombre_cliente
FROM cliente
WHERE codigo_cliente IN (
    SELECT codigo_cliente
    FROM pedido
)
AND codigo_cliente NOT IN (
    SELECT codigo_cliente
    FROM pago
);
```
<p align="center">
<img src="./imagenes/subconsulta 17.png" alt="subconsulta " width="200"/>
  
## Subconsultas con EXISTS y NOT EXISTS
18. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql
SELECT nombre_cliente
FROM cliente c
WHERE NOT EXISTS (
    SELECT 1
    FROM pago p
    WHERE p.codigo_cliente = c.codigo_cliente
);
```
<p align="center">
<img src="./imagenes/subconsulta 18.png" alt="subconsulta " width="200"/>
  
19. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql
SELECT nombre_cliente
FROM cliente c
WHERE EXISTS (
    SELECT 1
    FROM pago p
    WHERE p.codigo_cliente = c.codigo_cliente
);
```
<p align="center">
<img src="./imagenes/subconsulta 19.png" alt="subconsulta " width="200"/>
  
20. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql
SELECT nombre
FROM producto pr
WHERE NOT EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.codigo_producto = pr.codigo_producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 20.png" alt="subconsulta " width="300"/>
  
21. Devuelve un listado de los productos que han aparecido en un pedido alguna vez.
```sql
SELECT nombre
FROM producto pr
WHERE EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.codigo_producto = pr.codigo_producto
);
```
<p align="center">
<img src="./imagenes/subconsulta 21.png" alt="subconsulta " width="200"/>
  
