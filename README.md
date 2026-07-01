# Proyecto QUBO-QAOA: matching bipartito 4x4

## Dataset

**Nombre del dataset:** Avance de producción de caña de azúcar
(INFOCAÑA)
Información estadística de los principales indicadores de producción de 
la zafra
original: Infocana_25_26_resumen.csv
usado en el proyecto: dataset_real_4x4.csv

**Fuente oficial o confiable:** Plataforma Nacional de Datos Abiertos
https://www.datos.gob.mx/

**Institución responsable:** 
Comité Nacional para el Desarrollo Sustentable de la Caña de Azúcar 
(CONADESUCA) 

**URL de la fuente:**
https://www.datos.gob.mx/dataset/avance_produccion_cana_azucar_infocana
https://repodatos.atdt.gob.mx/api_update/conadesuca/avance_produccion_cana_azucar_infocana/Infocana_25_26_resumen.csv

**URL raw del CSV usado en `data/`:** 
https://raw.githubusercontent.com/graphene-zz/qubitmx/main/data/dataset_real_4x4.csv

**Licencia o condiciones de uso:** N/A

**Fecha de consulta:** 30-Jun-2026

**Dominio del problema:** Asignación óptima de paquetes de ayuda a
ingenios azucareros mediante un modelo de matching bipartito formulado
como QUBO.

------------------------------------------------------------------------

## Modelado

**Conjunto A:** - A1: Mejora del rendimiento en campo - A2: Mejora del
desempeño en fábrica - A3: Mejora de proceso - A4: Mejora integral

**Criterio para elegir exactamente 4 elementos de A:** Se definieron
cuatro tipos de intervención, uno por indicador. 
(Sólo hay una columna con un elemento de A por renglón)

**Conjunto B:** Cuatro ingenios azucareros.

**Criterio para elegir exactamente 4 elementos de B:** 
Los cuatro primeros ingenios azucareros seleccionados de la 
última semana de zafra, ordenados alfabéticamente por el nombre del ingenio.

**Definición de x_ij = 1:** El paquete A_i se asigna al ingenio B_j.

**Interpretación de x_ij = 0:** El paquete A_i no se asigna al ingenio
B_j.

------------------------------------------------------------------------

## Matriz de score

**Columnas usadas:** - rendimiento_campo - rendimiento_fabrica -
eficiencia_fabrica - rendimiento_agroindustrial

**Fórmula exacta de S_ij:**

S_ij=(v_ij-v_i^min)/(v_i^max-v_i\^min)

donde v_i\^min=min_j(v_ij) y v_i\^max=max_j(v_ij).

**Normalización aplicada:** Min-Max sobre los cuatro ingenios
seleccionados.

**Matriz S 4x4**

  Paquete  Ingenio         B1       B2       B3       B4
  ------------------ -------- -------- -------- --------
  A1                   0.0000   0.0471   1.0000   0.4661
  A2                   0.0000   0.5380   1.0000   0.3022
  A3                   0.2563   0.1537   1.0000   0.0000
  A4                   0.0000   0.1907   1.0000   0.3396

------------------------------------------------------------------------

## Restricciones

**Restricción por filas:** Σ_j x_ij = 1.

**Restricción por columnas:** Σ_i x_ij = 1.

**Otras restricciones:** No se consideran capacidades adicionales.

**Justificación de por qué el problema es matching bipartito:** Dos
conjuntos disjuntos (paquetes e ingenios) con asignaciones únicamente
entre conjuntos.

**Justificación de por qué es razonable modelarlo como QUBO:** El
objetivo y las restricciones uno-a-uno se expresan mediante una función
cuadrática con penalizaciones.

------------------------------------------------------------------------

## Resultados

**Solución clásica exacta:** 
id 	B1 	B2 	B3 	B4
 				
A1 	0 	0 	0 	1
A2 	0 	1 	0 	0
A3 	1 	0 	0 	0
A4 	0 	0 	1 	0

	A 	B 	score
0 	A1 	B4 	0.466072
1 	A2 	B2 	0.538009
2 	A3 	B1 	0.256294
3 	A4 	B3 	1.000000


**Resultado QAOA local:** 
id 	B1 	B2 	B3 	B4
 				
A1 	0 	0 	1 	0
A2 	0 	1 	0 	0
A3 	1 	0 	0 	0
A4 	0 	0 	0 	1

	A 	B 	score
0 	A1 	B3 	1.000000
1 	A2 	B2 	0.538009
2 	A3 	B1 	0.256294
3 	A4 	B4 	0.339572


**Comparación clásico vs QAOA local:** 
 	método 	energía 	score 	factible 	probabilidad_factible 	probabilidad_óptimo
0 	Clásico exacto 	-2.260375 	2.260375 	True 	NaN 	NaN
1 	QAOA local: mejor muestra 	-2.133875 	2.133875 	True 	0.023316 	0.000996

**Si se usó hardware real o pipeline híbrido:** Sí
qiskit_runtime_service._discover_account:WARNING:2026-07-01 04:58:08,108: Loading account with the given token. A saved account will not be used.

Backend: ibm_fez
Job ID: d929spr57qjs73b7ktl0
Estado inicial: QUEUED
Profundidad ISA: 446
Operaciones ISA: {'sx': 545, 'rz': 425, 'cz': 255, 'measure': 16, 'barrier': 1}
Estado final: DONE
Shots recibidos: 512
Bitstrings distintos: 480


Reparación local disponible: True
Reparación hardware disponible: True

Tabla comparativa de métodos

 	método 	shots 	energía_mejor_observada 	energía_media_muestreo 	score_mejor_observado 	score_medio_muestreo 	factible_mejor_observado 	probabilidad_factible 	probabilidad_óptimo_clásico 	bitstring_mejor
0 	Clásico exacto 	NaN 	-2.260375 	NaN 	2.260375 	NaN 	True 	NaN 	NaN 	0100000100101000
1 	QAOA local 	2000.0 	-2.133875 	17.639481 	2.133875 	1.645519 	True 	0.019000 	0.000000 	1000000100100100
2 	QAOA local reparado 	2000.0 	-2.260375 	-2.203027 	2.260375 	2.203027 	True 	1.000000 	0.634500 	0100000100101000
3 	Hardware real 	512.0 	-2.260375 	30.439654 	2.260375 	2.353315 	True 	0.021484 	0.001953 	0100000100101000
4 	Hardware real reparado 	512.0 	-2.260375 	-2.204540 	2.260375 	2.204540 	True 	1.000000 	0.642578 	0100000100101000

Curvas acumuladas, local vs hardware

método 	shots_acumulados 	mejor_energía_acumulada 	brecha_mejor_energía_% 	factibilidad_% 	óptimo_observado_%
0 	QAOA local 	1 	20.000000 	984.808860 	0.000000 	0.0
1 	QAOA local 	9 	7.656347 	438.720177 	0.000000 	0.0
2 	QAOA local 	17 	-1.538009 	31.957785 	11.764706 	0.0
3 	QAOA local 	25 	-1.538009 	31.957785 	8.000000 	0.0
4 	QAOA local 	33 	-1.538009 	31.957785 	9.090909 	0.0

Lectura automática local vs hardware

Lectura automática:
- Hardware real igualó o mejoró la mejor energía observada por QAOA local.
- Hardware real tuvo igual o mayor proporción de muestras factibles que QAOA local.
- Hardware real observó el óptimo clásico con igual o mayor frecuencia que QAOA local.

Lectura con reparación clásica:
- Hardware reparado igualó o mejoró la mejor energía local reparada.

Regla de reporte:
- Contra el clásico exacto, QAOA y hardware solo pueden empatar el óptimo en esta instancia pequeña.
- Si la mejora aparece solo después de reparación, debe reportarse como mejora del pipeline híbrido, no del hardware aislado.

------------------------------------------------------------------------

## Ética y limitaciones

**Riesgos éticos:** El modelo simplifica una decisión de asignación de
recursos y no debe utilizarse como criterio único.

**Medidas de mitigación:** Uso de datos públicos y construcción
transparente del score.

**Limitaciones del modelo:** - Instancia 4×4. - Normalización relativa
al subconjunto. - Sin restricciones presupuestales u operativas.

------------------------------------------------------------------------

## Ejecución

1.  Abrir el notebook en Google Colab.
2.  Verificar que `data/dataset_real_4x4.csv` esté disponible o
    accesible mediante su URL raw.
3.  modificar la línea: DATASET_CSV_PATH = Path("data/dataset_real_4x4.csv")
    del punto 14, sustituir por url raw del archivo.
4.  Ejecutar colab celda por celda.

## Preguntas

¿Cuál fue la mejor asignación encontrada?
Paquete									Ingenio
A1: Mejora del rendimiento en campo		B4: Bellavista
A2: Mejora del desempeño en fábrica		B2: Alianza Popular
A3: Mejora de proceso					B1: Adolfo López Mateos
A4: Mejora integral						B3: Atencingo

¿Cuál fue su score en el dominio?
El score total fue:
2.2603751964
Como el modelo está formulado como minimización, este valor corresponde a la menor suma de scores normalizados encontrada bajo las restricciones.

¿La asignación cumple todas las restricciones?
Sí. Cada paquete se asigna exactamente una vez y cada ingenio recibe exactamente un paquete.

¿QAOA local observó el óptimo clásico?
Si QAOA encontró la misma asignación, pero con valores un poco distintos

¿Qué tan frecuente fue observar soluciones factibles?
Poco frecuente

¿Qué limitaciones tiene el modelo 4×4?
El modelo es una instancia pequeña y simplificada. Sólo considera cuatro paquetes y cuatro ingenios, no incluye presupuesto, capacidad real de implementación, ubicación geográfica, costos, tiempos, impacto social ni restricciones operativas. Además la normalización produce muchos valores cercanos al cero o al uno.

¿Qué cambiaría si el dataset creciera?
Aumentaría el número de variables binarias. Si hubiera m paquetes y n ingenios, habría m×n variables. El QUBO y el circuito QAOA serían más grandes, aumentando el costo computacional y la dificultad de ejecutarlo en hardware real.

¿Qué riesgos éticos existen y cómo se mitigaron?
El riesgo principal es interpretar el modelo como una decisión automática de asignación de recursos. Se mitigó usando datos públicos agregados, explicando la fórmula de score y presentando el resultado como una herramienta exploratoria, no como una recomendación definitiva.

Si se usó hardware real, ¿cómo compara contra QAOA local?
Se ejecutó en ibm_fez con 512 shots. El hardware real encontró el óptimo clásico con energía -2.260375 y score 2.260375, mientras que QAOA local sin reparación obtuvo como mejor muestra energía -2.133875. Además, el hardware tuvo mayor probabilidad de factibilidad (2.15%) que QAOA local (1.90%) y sí observó el óptimo clásico (0.195%), mientras que QAOA local no lo observó directamente.
