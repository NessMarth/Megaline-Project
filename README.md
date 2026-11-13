 ¿Cuál es la mejor tarifa? Análisis de ingresos de Megaline

Análisis de 500 clientes de un operador de telecomunicaciones para determinar cuál de sus dos tarifas de prepago, **Surf** o **Ultimate**, genera más ingresos. Incluye limpieza de datos, cálculo de facturación, análisis exploratorio y pruebas de hipótesis.

> **Hallazgo principal:** por cliente, Ultimate genera más ($72.31 vs $60.71 por mes). En el total de la muestra, Surf suma más porque tiene más del doble de usuarios. Los clientes consumen casi igual en ambas tarifas; lo que cambia el ingreso es la estructura de cobro.

## 🎯 Objetivo

El departamento comercial necesita saber qué tarifa es más rentable para ajustar el presupuesto de publicidad.

## 📊 Datos

- **Fuente:** caso de estudio del programa de Ciencia de Datos de [TripleTen](https://tripleten.com/) (datos proporcionados por el curso, no incluidos en el repo).
- **Contenido:** 5 tablas (`users`, `calls`, `messages`, `internet`, `plans`) con datos de 500 clientes en 2018.
- **Análisis final:** 2,293 registros usuario-mes (1,573 de Surf y 720 de Ultimate).

## 🔍 Metodología

1. **Limpieza:** conversión de fechas, estandarización de texto, conversión de MB a GB, revisión de nulos y duplicados.
2. **Facturación:** cálculo del ingreso mensual por usuario según las reglas de cobro (pago base + excedentes). Cada llamada se redondea hacia arriba y el Internet se suma por mes antes de redondear a GB.
3. **Análisis exploratorio:** comparación por tarifa de minutos, llamadas, mensajes y GB.
4. **Pruebas de hipótesis:** t de Welch (α = 0.05) para comparar ingresos entre tarifas y entre regiones.

## ✅ Resultados clave

| | Surf | Ultimate |
|---|---|---|
| Ingreso promedio por usuario-mes | **$60.71** | **$72.31** |
| Varianza del ingreso | 3,067.8 | 129.9 |
| Minutos promedio al mes | 436.5 | 434.7 |
| GB promedio al mes | 16.7 | 17.3 |
| Registros que superan el Internet incluido | 57.9% | 5.7% |
| Registros usuario-mes | 1,573 | 720 |

- **Surf es volátil:** el 72.6% de sus registros paga más de la tarifa base de $20, sobre todo por Internet.
- **Ultimate es estable:** el 94% de sus registros paga solo los $70 base.
- **Prueba 1 (Surf vs. Ultimate):** p ≈ 3.2 × 10⁻¹⁵, la diferencia de ingreso es estadísticamente significativa.
- **Prueba 2 (NY-NJ vs. otras regiones):** p = 0.034, resultado marginal que debe tomarse con cautela.

![Distribución de ingresos por plan](reports/ingresos_por_plan.png)

![Registros usuario-mes que superan el límite de GB](reports/excedentes_internet_por_plan.png)

*Nota: la gráfica cuenta registros usuario-mes, no usuarios únicos.*

![Consumo mensual de Internet por plan](reports/consumo_internet_boxplot.png)

## 💡 Conclusiones

- Si el objetivo es **más ingreso por cliente**, la evidencia favorece a **Ultimate**.
- Si el objetivo es **más ingreso total con la base actual**, **Surf** pesa más por su volumen.
- Antes de decidir el presupuesto de publicidad conviene sumar el costo de adquisición por tarifa.

## ⚠️ Limitaciones

- Muestra de 500 clientes y un solo año.
- No se analizó la cancelación de clientes (`churn_date`) ni los costos de adquisición.
- La diferencia por región no controla el efecto de la tarifa ni de otras variables.

## ▶️ Cómo reproducirlo

```bash
git clone https://github.com/NessMarth/megaline-analisis-tarifas.git
cd megaline-analisis-tarifas
pip install -r requirements.txt
jupyter notebook notebooks/analisis_tarifas_megaline.ipynb
```

Se necesitan los CSV del curso en `data/` (ver [data/README.md](data/README.md)). El notebook se ejecutó con pandas 1.x; con pandas 2.0 o superior los formatos de fecha del código darán error, por eso `requirements.txt` lo limita.
