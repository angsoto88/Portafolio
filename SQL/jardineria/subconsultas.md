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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>

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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
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
<img src="./imagenes/tabla .png" alt="subconsulta " width="300"/>
  
