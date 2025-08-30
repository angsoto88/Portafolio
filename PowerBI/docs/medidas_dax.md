# 📐 Medidas DAX utilizadas en el Dashboard

Este archivo documenta las fórmulas DAX utilizadas para construir los KPIs, comparaciones temporales, márgenes y rankings del dashboard de ventas de PC Store.

---

## 🔢 Métricas básicas

- `Ventas = SUM(FACT_Ventas[CANTIDAD_PRODUCTOS])`  
  Total de productos vendidos.

- `# ventas = CALCULATE(COUNT(FACT_Ventas[COD_VENTA]))`  
  Número total de transacciones.

- `IngresosX = SUMX(FACT_Ventas, FACT_Ventas[CANTIDAD_PRODUCTOS] * FACT_Ventas[Precio unitario CC])`  
  Ingresos totales por producto.

- `CostosX = SUMX(FACT_Ventas, FACT_Ventas[CANTIDAD_PRODUCTOS] * FACT_Ventas[Costo unitario CC])`  
  Costos totales.

- `Margen = [IngresosX] - [CostosX]`  
  Ganancia bruta.

- `%Margen = IFERROR(DIVIDE([Margen], [IngresosX]), 0)`  
  Porcentaje de margen.

- `Precio Medio = DIVIDE([IngresosX], [Ventas], 0)`  
  Precio promedio por unidad.

- `venta_promedio = DIVIDE([IngresosX], [# ventas], 0)`  
  Ingreso promedio por transacción.

- `Fecha Actualización = TODAY()`  
  Fecha actual.

---

## 📈 Métricas temporales

- `Ventas Año Anterior = IF(YEAR(MAX('Tabla Calendario'[Fecha])) = 2020, BLANK(), CALCULATE([Ventas], DATEADD('Tabla Calendario'[Fecha], -1, YEAR)))`  
  Ventas del año anterior.

- `Ventas- ventas año anterior = IF(YEAR(MAX('Tabla Calendario'[Fecha])) = 2020, BLANK(), [Ventas] - [Ventas Año Anterior])`  
  Diferencia absoluta.

- `% Ventas- Ventas = IF(YEAR(MAX('Tabla Calendario'[Fecha])) = 2020, BLANK(), DIVIDE([Ventas] - [Ventas Año Anterior], [Ventas Año Anterior]))`  
  Variación porcentual.

---

## 🧮 Métricas acumulativas y ranking

- `RankingXCategoria = RANKX(ALL(FACT_Ventas[CategoriaCC]), [Ventas])`  
  Ranking por categoría.

- `%Acumulado = ...`  
  Calcula el porcentaje acumulado según el ranking.

- `Acumulado = ...`  
  Suma acumulada de ventas por categoría.

*(Ver el archivo `.pbix` para la lógica completa de estas medidas.)*
