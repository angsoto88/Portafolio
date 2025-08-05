## Consultas sobre una tabla
1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.
```sql
SELECT codigo_oficina, ciudad FROM oficina
```
2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.
```sql
SELECT ciudad, telefono FROM oficina WHERE pais = 'España'
```
3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.
```sql
SELECT nombre, apellido1, email FROM empleado WHERE codigo_jefe = '7'
```
4 .Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.
```sql
SELECT puesto, nombre, apellido1, apellido2, email FROM empleado WHERE puesto = 'Director General'
```
5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.
```sql
SELECT nombre, apellido1, apellido2, puesto FROM empleado WHERE puesto != 'Representante Ventas'
```
6. Devuelve un listado con el nombre de los todos los clientes españoles.
```sql
SELECT nombre_cliente FROM cliente WHERE pais = 'Spain'
```
7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.
```sql
SELECT DISTINCT estado FROM pedido
```
8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
8.1. Utilizando la función YEAR de MySQL.
```sql
SELECT DISTINCT codigo_cliente
FROM jardineria.pago
WHERE YEAR(fecha_pago) = 2008;
```
8.2. Utilizando la función DATE_FORMAT de MySQL.
```sql
SELECT DISTINCT codigo_cliente
FROM jardineria.pago
WHERE DATE_FORMAT(fecha_pago, '%Y') = '2008';
```
8.3. Sin utilizar ninguna de las funciones anteriores.
```sql
SELECT DISTINCT codigo_cliente
FROM jardineria.pago
WHERE fecha_pago BETWEEN '2008-01-01' AND '2008-12-31';
```
9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.
```sql
SELECT  codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
FROM pedidos
WHERE fecha_entrega > fecha_esperada;
```
10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.
10.1. Utilizando la función ADDDATE de MySQL.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
FROM jardineria.pedido
WHERE fecha_entrega <= ADDDATE(fecha_esperada, INTERVAL -2 DAY);
```
10.2. Utilizando la función DATEDIFF de MySQL.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
FROM jardineria.pedido
WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2;
```
10.3 ¿Sería posible resolver esta consulta utilizando el operador de suma + o resta -?
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
FROM jardineria.pedido
WHERE fecha_entrega <= fecha_esperada - INTERVAL 2 DAY;
```
11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_pedido, estado
FROM pedido
WHERE estado = 'Rechazado' AND YEAR(fecha_pedido) = 2009;
```
12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_pedido, estado
FROM pedido 
WHERE estado = 'Entregado' AND MONTH(fecha_pedido) = 1;
```
13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.
```sql
SELECT id_transaccion, fecha_pago, forma_pago, total
FROM pago
WHERE forma_pago = 'Paypal' AND YEAR(fecha_pago) = 2008
ORDER BY total DESC;
```
14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.
```sql
SELECT DISTINCT forma_pago
FROM pago;
```
15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.
```sql
SELECT codigo_producto, nombre, gama, precio_venta, cantidad_en_stock
FROM jardineria. producto
WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100
ORDER BY precio_venta DESC;
```
16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.
```sql
SELECT codigo_cliente, nombre_cliente, ciudad, codigo_empleado_rep_ventas
FROM jardineria.cliente
WHERE ciudad = 'Madrid' AND codigo_empleado_rep_ventas IN (11, 30);
```
