# Habnetic — Phase 4 / M0: Rotterdam baseline reproducibility

**Fecha:** 22 de octubre de 2026  
**Proyecto:** Resilient Housing Bayes / Habnetic  
**Hito:** M0 — Recuperación, auditoría y reproducción de Phase 3 en Rotterdam (RTM)  
**Estado:** **Baseline operativo verificado para RTM**. Quedan tareas de documentación y validación complementaria.  
**Uso previsto:** Nota de investigación y registro de decisiones para preparar M1.  
**Ubicación sugerida:** `docs/phase4/M0_RTM_Baseline.md`

## 1. Objetivo y alcance

Establecer una referencia **reproducible y cuantificada** de Phase 3 antes de cambiar el modelo de inundación. En concreto:

1. Congelar la versión del código y proteger los resultados originales.
2. Regenerar el preprocesamiento y los resultados sintéticos de RTM.
3. Repetir la inferencia bayesiana y verificar sus diagnósticos.
4. Recalcular la priorización posterior y contrastar estabilidad y selecciones.
5. Documentar las limitaciones científicas que M1–M4 deberán poner a prueba.

**Alcance:** Rotterdam, con los **mismos datos de entrada**, scaler y entorno Python disponibles en octubre de 2026. **No** se ha reproducido aquí el experimento completo de Hamburgo y Donostia, ni se ha demostrado portabilidad entre entornos.

## 2. Referencias reproducibles

| Elemento | Referencia / resultado |
|---|---|
| Commit congelado | `5e19254` — `Extract generic decision stability metrics` |
| Tag | `phase3-code-freeze-20261022` |
| Rama de trabajo | `phase4/m0-reproducibility` |
| Tests disponibles | **5/5 correctos** |
| Worktree de reproducción | `../phase3-m0-repro` (HEAD desacoplado en el tag) |
| Copia de referencia | `../phase3-m0-baseline-20261022/RTM` |
| Integridad del backup | **7/7 archivos** con SHA-256 igual al original |
| Fuente de activos | `../data/processed/RTM/phase3_assets.parquet` |
| Scaler conservado | `rtm_feature_scaler.json` |
| Entorno de ejecución | La misma `.venv` del repositorio principal; `pip freeze` exportado a `phase3-m0-repro/m0_environment.txt` |

Los siete archivos respaldados fueron `run_metadata.json`, `summary.json`, `decision_metrics.json`, `idata.nc`, `asset_metrics.parquet`, `phase3_features_scaled.parquet` y `rtm_feature_scaler.json`. El tag de Git **no** protege por sí solo los archivos no versionados, los datos externos ni los resultados de inferencia.

## 3. Modelo experimental de Phase 3

### Historia generativa

Para cada edificio \(i\), el resultado de daño es **sintético**. Se genera a partir de un proxy de exposición \(E_i\) y un predictor de hazard \(H_i\), utilizando una regresión logística con parámetros conocidos:

\[
Y_i \sim \operatorname{Bernoulli}(p_i),\qquad
\operatorname{logit}(p_i) = \alpha + \beta_E E_i + \beta_H H_i.
\]

Parámetros usados al **generar** los datos: \(\alpha=-3\), \(\beta_E=1\), \(\beta_H=1\). Semilla de generación: `20260430`. La inferencia estima estos parámetros a partir de los mismos resultados sintéticos; **no** utiliza registros observados de daños por inundación.

### Variables y escala

- **Exposición:** `E_hat_v0`, derivada de distancia al agua y densidad de agua en radios de 250, 500 y 1.000 m.
- **Hazard:** `H_pluvial_v1_logrel`, logaritmo de una razón respecto de la mediana de `H_pluvial_v1_mm` en RTM (mediana registrada: **25,6437 mm**).
- **Objetivo:** `Y_damage`, indicador binario sintético.
- **Unidad de decisión:** edificio identificado mediante `bldg_id`.

### Likelihood, priors e inferencia

\[
Y_i\mid \alpha,\beta_E,\beta_H,E_i,H_i \sim
\operatorname{Bernoulli}\bigl(\operatorname{logit}^{-1}(\alpha+\beta_EE_i+\beta_HH_i)\bigr).
\]

Priors independientes: \(\alpha,\beta_E,\beta_H \sim \mathcal N(0,2.5^2)\). Se utiliza PyMC/NUTS con **4 cadenas**, **1.000 iteraciones de ajuste y 1.000 draws por cadena**, `target_accept=0.9`, semilla de muestreo `42` y posterior predictive con semilla `42`.

### Cantidades de decisión

Para cada draw posterior se calcula el riesgo de cada edificio y se seleccionan los \(k\) edificios con mayor puntuación. La probabilidad de pertenencia al top-k es la frecuencia de selección a través de draws. Las clases operativas originales son:

- **Inclusión estable:** \(P(\text{top-k})\ge 0,8\).
- **Borderline:** \(0,2<P(\text{top-k})<0,8\).
- **Exclusión estable:** \(P(\text{top-k})\le 0,2\).

Estos umbrales son convenciones de decisión, **no** garantías de corrección predictiva.

## 4. Resultados de reproducibilidad

### 4.1 Preprocesamiento: igualdad numérica exacta

| Control | Resultado |
|---|---:|
| Filas | **221.324** en ambos archivos |
| Columnas | **16** en ambos archivos |
| Comparación de valores (`check_exact=True`, ignorando solo el dtype) | **PASS** |
| Eventos sintéticos `Y_damage=1` | **13.953** en ambos |
| Tasa de evento sintético | **6,3043 %** |

**Única discrepancia de esquema detectada:** `Y_damage` es `int32` en el archivo original e `int64` en el regenerado. Los **valores coinciden exactamente**; no se afirma identidad binaria de los Parquet. El motivo del cambio de dtype no se ha identificado.

### 4.2 Inferencia: concordancia estadística

| Parámetro | Media original | SD original | Media reproducida | SD reproducida |
|---|---:|---:|---:|---:|
| \(\alpha\) | -2,983 | 0,011 | -2,983 | 0,010 |
| \(\beta_E\) | 0,985 | 0,010 | 0,985 | 0,010 |
| \(\beta_H\) | 0,434 | 1,137 | 0,492 | 1,140 |

- **0 divergencias** en la repetición; \(\widehat R=1,00\) para los tres parámetros.
- ESS bulk reproducido: \(\alpha=2543\), \(\beta_E=2610\), \(\beta_H=2985\). ESS tail: **2701, 2714, 2570**, respectivamente.
- El intervalo HDI del 94 % para \(\beta_H\) fue **[-1,550, 2,697]** originalmente y **[-1,614, 2,696]** en la reproducción.

**Interpretación:** el posterior se ha reproducido con **concordancia estadística**, no con draws idénticos. Una buena convergencia MCMC no implica identificación precisa de todos los coeficientes.

### 4.3 Prioridades y estabilidad

| k | Borderline original | Borderline reproducido | Diferencia |
|---:|---:|---:|---:|
| 1.000 | 8 | 8 | 0 |
| 1.107 | 12 | 14 | +2 |
| **2.213 (≈1 %)** | **30** | **28** | **-2** |
| 2.500 | 38 | 35 | -3 |
| 5.000 | 103 | 107 | +4 |
| 5.533 | 98 | 99 | +1 |

Para **k=2.213**, al comparar cada edificio por `bldg_id`:

- Diferencia absoluta **media** entre probabilidades de pertenencia: **0,00000454**.
- Diferencia absoluta **máxima**: **0,02725**.
- Edificios con diferencias superiores a 0,05: **0**.
- Cambios de clase (exclusión / borderline / inclusión): **2**.
- Selección basada en probabilidad media posterior: **2.213 de 2.213 edificios coinciden (100 %)**.

Además, las sumas originales de las probabilidades de pertenencia cumplieron el invariante \(\sum_i P(i\in\mathrm{top}\text{-}k)=k\) para los seis valores de \(k\). Se comprobó el mismo invariante en la comparación reproducida para \(k=2213\).

**Importante:** la coincidencia del conjunto seleccionado por `p_mean` **no equivale** a identidad de todas las probabilidades de top-k ni garantiza que la selección sea físicamente correcta.

### 4.4 Posterior predictive check (PPC)

Comprobación realizada sobre la **tasa agregada de daño sintético** con 4.000 simulaciones predictivas:

| Estadística | Resultado |
|---|---:|
| Tasa sintética observada | **0,063043** |
| Media predictiva posterior | **0,063052** |
| SD predictiva | **0,000696** |
| Intervalo predictivo central del 95 % | **[0,061670; 0,064385]** |
| Tasa observada dentro del intervalo | **Sí** |

El modelo reproduce esta **estadística agregada**. No demuestra calibración por edificio, rendimiento fuera de muestra ni capacidad de predecir daños reales.

## 5. Qué aprendimos

### A. La selección de Phase 3 está casi completamente dominada por la exposición

Distribuciones de entrada en RTM:

| Variable | Media | SD | Correlación con la otra |
|---|---:|---:|---:|
| `E_hat_v0` | ≈ 0 | **0,832326** | 0,032251 |
| `H_pluvial_v1_logrel` | 0,000943 | **0,007567** | 0,032251 |

La variación del hazard en las unidades del modelo es unas **110 veces menor** que la de exposición. La correlación entre ambas variables es pequeña; la colinealidad lineal no parece la explicación principal de la escasa precisión de \(\beta_H\).

Comparación entre un ranking de **solo exposición** y el ranking por **probabilidad media posterior de daño**:

| k | Coincidencias | Solapamiento |
|---:|---:|---:|
| 1.000 | 999 | **99,90 %** |
| 2.213 | 2.209 | **99,82 %** |
| 5.000 | 4.987 | **99,74 %** |

Esta comparación **no** es una prueba causal ni un análisis de todas las muestras posteriores. Sí muestra que el hazard aporta poca diferenciación al ranking medio de este experimento.

### B. La identificación de \(\beta_H\) es débil, aunque NUTS converge bien

El valor generador de \(\beta_H=1\) está dentro del intervalo posterior, pero su SD ≈ **1,14** es elevada. Una explicación plausible es la escasa variabilidad del predictor de hazard y, por tanto, su limitada capacidad para informar el coeficiente. Esta explicación todavía debe contrastarse con experimentos de sensibilidad o recuperación de parámetros; **no** se ha establecido una causa única.

**No basta con estandarizar H y conservar los mismos priors:** reescalar un predictor no crea información y, si el prior de su coeficiente no se transforma coherentemente, cambia el modelo.

### C. Estabilidad de decisiones ≠ validez del riesgo

Un top-k reproducible y poco cambiante puede surgir de una variable dominante y un mecanismo generador sintético relativamente simple. La estabilidad está **condicionada a los datos, priors, modelo, selección bajo k y fuentes de incertidumbre incluidas**. No prueba que el ranking priorice correctamente los edificios ante inundaciones reales.

En \(k=2213\), los **30 borderline originales** equivalen a **0,01355 % de todos los edificios**, pero a **1,36 % de la capacidad k**. Ambas normalizaciones son útiles; la primera, por sí sola, puede hacer parecer trivial la incertidumbre en la frontera de decisión.

### D. El núcleo de decisión es reutilizable, con límites conocidos

`src/rhb/decision/topk.py` y `stability.py` trabajan con arrays de puntuaciones y probabilidades; no contienen lógica específica de inundaciones. Esto facilita probar otro dominio en el futuro.

Deuda técnica identificada: manejo explícito de empates, validación de entradas y \(k\), análisis de sensibilidad a umbrales 0,2/0,8 y reducción de memoria. El cálculo original materializa una matriz completa de probabilidades y otra de rankings, con un coste elevado para ciudades grandes.

## 6. Limitaciones y tareas abiertas

1. **Observaciones:** los daños son sintéticos. No se ha realizado validación con eventos, inundación observada o daños verificados.
2. **Incertidumbre de entradas:** se propaga incertidumbre de parámetros, pero \(E_i\) y \(H_i\) se tratan como fijos en el cálculo de posterior de decisiones.
3. **PPC limitado:** se verificó la tasa global, no calibración espacial/individual ni rendimiento fuera de muestra.
4. **Priors y sensibilidad:** existen artefactos previos predictivos en Phase 3, pero en esta auditoría **no se repitieron ni evaluaron sistemáticamente** prior predictive checks, priors alternativos ni likelihoods alternativos.
5. **Reproducibilidad ambiental:** la repetición utilizó la **misma `.venv`**. Queda fijar versiones y comprobar en un entorno independiente.
6. **Proveniencia:** falta inventariar hashes/versiones de los datos de entrada y archivar un manifiesto completo de ejecución. Los archivos de respaldo seleccionados sí fueron verificados por SHA-256.
7. **Cobertura:** M0 auditó y reprodujo RTM, **no** el conjunto de ciudades de Phase 3.
8. **Algoritmo:** la igualdad de masa top-k y los 5 tests existentes son buenas verificaciones parciales, pero no una prueba exhaustiva de corrección frente a empates, límites o entradas inválidas.

Estas tareas son **pendientes de calidad y trazabilidad**; no invalidan el cierre de M0 como referencia operativa de RTM. Deben abordarse antes de afirmar reproducibilidad completa en una publicación o entrega externa.

## 7. Cómo repetir el experimento

**Datos esperados** (fuera del repositorio): `../data/processed/RTM/phase3_assets.parquet`. **Configuración:** copiar el scaler original a `../phase3-m0-repro/outputs/phase3/config/rtm_feature_scaler.json`. **Código:** worktree fijado al tag `phase3-code-freeze-20261022`. **Entorno:** `.venv` del repositorio principal.

Desde PowerShell en la raíz de `resilient-housing-bayes`, **con el worktree ya creado y el scaler colocado**, esta secuencia vuelve a generar el baseline en **el directorio de reproducción**:

```powershell
$repo = (Get-Location).Path
$work = (Resolve-Path ..\phase3-m0-repro).Path
$python = Join-Path $repo '.venv\Scripts\python.exe'
$previousPythonPath = $env:PYTHONPATH

try {
    $env:PYTHONPATH = Join-Path $work 'src'
    Push-Location $work
    try {
        & $python -m rhb.phase3_transfer --city RTM
        if ($LASTEXITCODE -ne 0) { throw 'Preprocessing failed' }
        & $python -m rhb.pipelines.run_phase3_model --city RTM
        if ($LASTEXITCODE -ne 0) { throw 'Inference failed' }
        & $python -m rhb.pipelines.compute_phase3_decision_metrics --city RTM
        if ($LASTEXITCODE -ne 0) { throw 'Decision metrics failed' }
    }
    finally { Pop-Location }
}
finally { $env:PYTHONPATH = $previousPythonPath }
```

**Precaución:** la secuencia sobrescribe archivos **dentro del worktree de reproducción**, por lo que conviene preservar previamente sus resultados si se necesitan para otra comparación. No ejecutar los scripts originales desde el repositorio principal si se desea conservar `outputs/phase3/RTM` intacto.

**Salidas:** `phase3_features_scaled.parquet`, `idata.nc`, `summary.json`, `asset_metrics.parquet`, `decision_metrics.json` y metadatos en `phase3-m0-repro/outputs/phase3/RTM/`.

**Verificación mínima:** comparación exacta de valores del DataFrame (permitiendo la diferencia documentada `int32/int64`); comparación de medias/SD/HDI y diagnósticos MCMC; contraste de borderline por k; alineamiento por `bldg_id`, probabilidad top-k y solapamiento de selección; PPC de tasa agregada. **No exigir coincidencia bit a bit del posterior MCMC.**

## 8. Decisiones para M1

- **Conservar M0 sin cambios** como control fijo y no optimizar ni reescalar sus predictores a posteriori para mejorar los resultados.
- **Primera hipótesis de M1:** información topográfica justificable físicamente puede modificar la priorización actualmente dominada por exposición. La magnitud, dirección y utilidad de ese cambio son preguntas abiertas.
- Separar dos métricas de éxito: **(a) cambio/robustez de la decisión** y **(b) plausibilidad o validez del riesgo**. Que cambie el ranking no significa automáticamente que mejore.
- Incorporar **una sola modificación controlada** en M1, con calidad de datos, unidades, referencia topográfica y sensibilidad documentadas. Sin daños observados, tratar el efecto del terreno como **escenario o sensibilidad**, no como validación predictiva.
- Mantener el **motor posterior → top-k** independiente del dominio; dejar la generalización a un segundo dominio para una comparación posterior.

## 9. What to test next

1. Registrar hashes de `phase3_assets.parquet`, el scaler y los artefactos críticos, junto con el entorno de paquetes y el comando de ejecución.
2. Convertir las comprobaciones de M0 en tests automatizados pequeños, especialmente conservación de masa top-k, empates, \(k\) inválidos y comparación alineada por identificador.
3. Revisar las distribuciones prior predictive y una variante de prior/likelihood para comprobar sensibilidad del modelo base.
4. Iniciar **M1: inventario y QA de topografía de Rotterdam**, formular una hipótesis física explícita y definir la comparación emparejada M0–M1.

---

**Conclusión:** M0 ha proporcionado un **baseline operativo reproducible** para Rotterdam: mismo preprocesamiento numérico, posterior estadísticamente concordante, selección del 1 % idéntica y PPC agregado satisfactorio. También ha mostrado que la estabilidad reportada en Phase 3 depende, en este experimento, de un ranking casi enteramente explicado por exposición y de daños sintéticos. Es una referencia útil para el siguiente experimento, **no** una demostración de validez física del riesgo.
