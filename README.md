# Procesos gaussianos aplicados a las exportaciones de yerba mate argentina (2002–2026)

¿Las exportaciones mensuales de yerba mate argentina tienen una **estacionalidad anual identificable**, o su variación mes a mes es sobre todo ruido alrededor de una tendencia lenta?

Este proyecto responde la pregunta modelando la serie (en logaritmo natural) con **regresión por procesos gaussianos** y comparando kernels que representan hipótesis distintas sobre la dependencia temporal.

## Datos

- **Variable:** volumen mensual de yerba mate elaborada exportada, en kg (posición NCM 0903.00.90).
- **Fuente:** [INDEC – Sistema de Consulta de Comercio Exterior de Bienes](https://comex.indec.gob.ar/).
- **Período:** enero de 2002 a septiembre de 2026 (297 observaciones, sin faltantes).
- **Archivo:** `datos/yerba_mate_export_2002_2026.csv` (columnas `fecha`, `export_kg`).

## Método

1. **Estructura:** se comparan cinco kernels (solo ruido, exponencial, RBF, Matérn 3/2 y Periódico + Matérn) mediante la verosimilitud marginal y el AIC.
2. **Magnitud:** con el kernel ganador se descompone la varianza en tendencia, ciclo anual y ruido.
3. **Utilidad:** validación fuera de muestra con ventana expansiva (79 orígenes, horizonte de hasta 12 meses), frente a la media histórica, naive, naive estacional y ARIMA(1,0,0). Se evalúan RMSE y cobertura de las bandas del 80 % y 95 %.
4. **Robustez:** análisis de sensibilidad a las cotas de los hiperparámetros y a los reinicios del optimizador.
5. **Pronóstico:** 12 meses (oct-2026 a sep-2027) con bandas predictivas.

## Resultados principales

| Aspecto | Resultado |
|---|---|
| Mejor kernel (AIC) | Periódico + Matérn, con ΔAIC ≈ 16 frente al siguiente |
| Varianza explicada por componente | Ruido 58 %, tendencia 29 %, ciclo anual 13 % |
| Mejora del RMSE fuera de muestra | ≈ 2 % frente al mejor modelo de referencia |
| Cobertura de la banda del 95 % | ≈ 91 % |

**Conclusión:** existe una estructura estacional anual identificable, pero explica una parte pequeña de la varianza y aporta poco al pronóstico. La distinción entre *detectar* un ciclo y que ese ciclo *sirva para predecir* es el punto central del análisis.

## Estructura del repositorio

```
├── informe/   Informe completo en PDF
├── codigo/    Notebook con todo el análisis (Python)
└── datos/     Serie mensual de exportaciones
```

## Cómo reproducirlo

```bash
pip install -r requirements.txt
jupyter notebook codigo/procesos_gaussianos_yerba_mate.ipynb
```

El notebook lee los datos desde `../datos/`, por lo que debe ejecutarse con `codigo/` como directorio de trabajo (el comportamiento por defecto de Jupyter). La validación fuera de muestra reajusta varios modelos en 79 orígenes y puede tardar unos minutos.

## Tecnologías

Python · scikit-learn (`GaussianProcessRegressor`) · statsmodels · NumPy · pandas · SciPy · Matplotlib

## Autor

**Ignacio Lopez Parra** – Data Analyst / Data Scientist
