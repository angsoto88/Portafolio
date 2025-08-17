# 📊 Consultas SQL sobre Modelo Relacional de Ventas

Este repositorio contiene una colección de consultas SQL diseñadas para explorar un modelo relacional que representa un sistema de ventas. Las consultas abordan distintos aspectos del negocio, como clientes, empleados, pagos, pedidos y productos.

## 🧠 Modelo Relacional

Las entidades principales del modelo son:

- `cliente`
- `empleado`
- `oficina`
- `pago`
- `pedido`
- `detalle_pedido`
- `producto`
- `gama_producto`

## 📌 Consultas y Resoluciones

1. Nombre de cada cliente y su representante de ventas
```sql
SELECT c.nombre_cliente,
       e.nombre AS nombre_rep,
       e.apellido1 AS apellido_rep
FROM cliente c
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```
<p align="center">
<img src="./imagenes/tabla 2.1.png" alt="consulta 1" width="300"/>


2. Clientes que hayan realizado pagos y sus representantes
```sql
SELECT DISTINCT c.nombre_cliente,
       e.nombre AS nombre_rep,
       e.apellido1 AS apellido_rep
FROM cliente c
JOIN pago p ON c.codigo_cliente = p.codigo_cliente
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```
<p align="center">
<img src="./imagenes/tabla 2.2.png" alt="consulta 1" width="300"/>

3. Clientes que no hayan realizado pagos y sus representantes
```sql
SELECT c.nombre_cliente,
       e.nombre AS nombre_rep,
       e.apellido1 AS apellido_rep
FROM cliente c
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
WHERE p.codigo_cliente IS NULL;
```
<p align="center">
<img src="./imagenes/tabla 2.3.png" alt="consulta 1" width="300"/>

4. Clientes con pagos, representantes y ciudad de oficina
```sql
SELECT DISTINCT c.nombre_cliente,
       e.nombre AS nombre_rep,
       e.apellido1 AS apellido_rep,
       o.ciudad
FROM cliente c
JOIN pago p ON c.codigo_cliente = p.codigo_cliente
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```
<p align="center">
<img src="./imagenes/tabla 2.4.png" alt="consulta 1" width="300"/>

5. Clientes sin pagos, representantes y ciudad de oficina
```sql
SELECT c.nombre_cliente,
       e.nombre AS nombre_rep,
       e.apellido1 AS apellido_rep,
       o.ciudad
FROM cliente c
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
WHERE p.codigo_cliente IS NULL;
```
<p align="center">
<img src="./imagenes/tabla 2.5.png" alt="consulta 1" width="300"/>

6. Dirección de oficinas con clientes en Fuenlabrada
```sql
SELECT DISTINCT o.linea_direccion1, o.linea_direccion2
FROM oficina o
JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
WHERE c.ciudad = 'Fuenlabrada';
```
<p align="center">
<img src="./imagenes/tabla 2.6.png" alt="consulta 1" width="300"/>

7. Clientes, representantes y ciudad de oficina
```sql
SELECT c.nombre_cliente,
       e.nombre AS nombre_rep,
       e.apellido1 AS apellido_rep,
       o.ciudad
FROM cliente c
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```
<p align="center">
<img src="./imagenes/tabla 2.7.png" alt="consulta 1" width="300"/>

8. Empleados y sus jefes
```sql
SELECT e.nombre AS empleado,
       j.nombre AS jefe
FROM empleado e
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado;
```
<p align="center">
<img src="./imagenes/tabla 2.8.png" alt="consulta 1" width="200"/>

9. Empleados, su jefe y el jefe de su jefe
```sql
SELECT e.nombre AS empleado,
       j.nombre AS jefe,
       jj.nombre AS jefe_del_jefe
FROM empleado e
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado
LEFT JOIN empleado jj ON j.codigo_jefe = jj.codigo_empleado;
```
<p align="center">
<img src="./imagenes/tabla 2.9.png" alt="consulta 1" width="200"/>

10. Clientes con pedidos entregados tarde
```sql
SELECT DISTINCT c.nombre_cliente
FROM cliente c
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
WHERE p.fecha_entrega > p.fecha_esperada;
```
<p align="center">
<img src="./imagenes/tabla 2.10.png" alt="consulta 1" width="200"/>

11. Gamas de producto compradas por cada cliente
```sql
SELECT DISTINCT c.nombre_cliente,
       gp.gama
FROM cliente c
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
JOIN detalle_pedido dp ON p.codigo_pedido = dp.codigo_pedido
JOIN producto pr ON dp.codigo_producto = pr.codigo_producto
JOIN gama_producto gp ON pr.gama = gp.gama;
```
<p align="center">
<img src="./imagenes/tabla 2.11.png" alt="consulta 1" width="200"/>

