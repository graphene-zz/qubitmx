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

**Si se usó hardware real o pipeline híbrido:** N/A

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
