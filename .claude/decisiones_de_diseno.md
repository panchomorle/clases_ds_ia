# Decisiones de diseño

Cada decisión tomada al implementar la entrega, con su justificación y la
parte de `consigna_agentes_movilidad.md` (idéntica al `.pdf`, verificado)
que la respalda. Sirve para defender el trabajo y para que quien lo revise
entienda por qué el código quedó así y no de otra forma.

## `agentes_movilidad.py`

### Campos requeridos para validar una percepción

Se definieron `presion` y `capacidad_x` como los dos campos mínimos que
deben existir y ser numéricos válidos (`CAMPOS_REQUERIDOS`). No se exigieron
todos los campos de `percepciones.csv` porque la consigna dice "faltan datos
**requeridos**" (no "cualquier dato"), y esos dos son los únicos que
participan directamente en las reglas de decisión (Parte 1 y 2 de la
consigna). Exigir de más habría hecho que el agente abstenga en casos donde
sí tiene información suficiente para decidir.

### "Capacidad desconocida" = `capacidad_x <= 0`

La consigna dice literalmente: *"la capacidad es desconocida → ABSTENERSE"*.
Se interpretó `capacidad_x <= 0` como "desconocida o nula" porque, en
`simulador_entorno_agente.py`, `capacidad_x = taxis_x * viajes_por_taxi`: si
la empresa tiene 0 taxis, la presión queda matemáticamente indefinida
(`construir_percepcion` devuelve `presion = inf` si hay demanda, o `0.0` si
no la hay) — no hay flota sobre la cual basar una recomendación de refuerzo.

### Validez numérica con `math.isfinite`

Se usó `math.isfinite` (no solo `is None`) para atrapar también `NaN` e
infinitos, que son los valores que puede producir el propio simulador
cuando `capacidad_x` es 0. Sin este chequeo, `presion >= 0.85` con
`presion = inf` daría `True` y el agente recomendaría refuerzo con datos
sin sentido, violando la regla de abstención.

### Reinicio de la racha ante percepción inválida

La consigna no dice explícitamente qué hacer con `racha_presion_alta`
cuando la percepción es inválida, solo que el estado debe quedar en una
condición que produzca `ABSTENERSE`. Se decidió resetear la racha a `0` en
ese caso (no conservar el valor anterior) porque un dato faltante rompe la
continuidad de la observación: no hay evidencia de que la presión se haya
mantenido alta durante ese hueco. Mantener la racha anterior hubiera sido
optimista sin sustento.

### Motivos (`motivo_simple`, `motivo_modelo`) como texto explicativo

La consigna pide una tupla `(accion, motivo)` y que la bitácora incluya
"justificación de la regla aplicada". Se optó por strings legibles que citan
el valor numérico exacto (presión o racha) en vez de un código genérico,
para que la trazabilidad (exigida explícitamente por la consigna y por el
criterio de evaluación PEAS/causalidad) sea verificable a simple vista sin
tener que re-ejecutar el código.

### Orden temporal explícito en `procesar_secuencia`

Se ordena por `hora` (`percepciones.sort_values("hora")`) antes de iterar,
en lugar de asumir que el DataFrame ya viene ordenado. Esto evita que un
CSV desordenado rompa la dependencia histórica del agente basado en modelo
(la consigna exige "ejecutar ambos agentes en orden temporal").

### Ningún acceso a `resultado_h_mas_1.csv`

Ninguna función del módulo importa, abre ni recibe como parámetro ese
archivo. Las firmas de `decidir_reactivo_simple(percepcion)` y
`decidir_reactivo_modelo(estado_actual)` solo aceptan el dato del instante
actual, lo cual se verifica con un test que inspecciona la firma
(`test_funciones_de_decision_no_reciben_datos_futuros`).

## `test_agentes_movilidad.py`

Se agregaron dos tests que la consigna no pide literalmente en la tabla de
"Pruebas obligatorias", pero que sí exige el resto del enunciado:

- `test_capacidad_desconocida_abstiene`
- `test_campo_faltante_abstiene`

Justificación: la tabla de reglas de la Parte 1 incluye explícitamente la
condición de abstención por datos faltantes/inválidos/capacidad desconocida,
y esa regla vale puntaje en la rúbrica ("Agente reactivo simple **y
validación de entradas**"). Sin un test de esto, esa mitad del criterio
quedaría sin comprobar.

También se agregó `test_procesar_secuencia_ignora_cambios_futuros`, más
fuerte que la inspección de firmas: corre la secuencia dos veces, cambia
solo el valor de la hora más "futura", y verifica que las decisiones de
horas anteriores no cambien. Esto demuestra la ausencia de fuga temporal de
forma empírica, no solo estructural.

## Generación del escenario y la bitácora

Se usó exactamente el comando de ejemplo de la consigna (zona `161`, hora
`8`, `taxis-x 20`, `horas-historia 3`, semilla `42`) en lugar de elegir
parámetros "más lindos" para mostrar más variedad de acciones. Motivo: la
consigna pide un "escenario reproducible", y ese es el único comando que
aparece documentado como ejemplo reproducible; cambiar los parámetros sin
necesidad hubiera sido injustificable ante una revisión.

`generar_bitacora.py` se escribió como script separado (no a mano ni en una
notebook) para que la bitácora sea regenerable con un solo comando y
verificable por cualquiera, alineado con el principio de reproducibilidad
que pide toda la consigna.

## Bug de Windows en `simulador_movilidad.py`

`cargar_centros_zonas` usaba:

```python
with tempfile.NamedTemporaryFile(suffix=".zip") as archivo_zip:
    urllib.request.urlretrieve(zonas_url, archivo_zip.name)
    ...
```

En Windows, `NamedTemporaryFile` abre el archivo con acceso exclusivo;
`urlretrieve` intenta volver a abrir esa misma ruta para escribir y falla
con `PermissionError`. En Linux/Mac esto no ocurre (por eso probablemente
nadie lo había visto antes en clase). Se reemplazó por:

```python
descriptor, ruta_zip = tempfile.mkstemp(suffix=".zip")
os.close(descriptor)
try:
    urllib.request.urlretrieve(zonas_url, ruta_zip)
    zonas = gpd.read_file(f"zip://{ruta_zip}!taxi_zones/taxi_zones.shp")
finally:
    os.remove(ruta_zip)
```

Es un cambio de compatibilidad puro: mismo comportamiento en cualquier
sistema operativo, misma fuente de datos, mismo resultado. No afecta
ningún cálculo ni la lógica de negocio del simulador. Se decidió corregirlo
en el archivo compartido (en vez de duplicar la función en un script propio)
porque así el comando oficial de la consigna (`simulador_entorno_agente.py`
con los flags documentados) sigue funcionando tal cual está descrito, sin
necesitar un script alternativo.

## Limitación agregada que no está en la consigna

Se documentó en `informe.md` un efecto de borde no mencionado por la
consigna: como `crear_estado_inicial()` siempre arranca con
`racha_presion_alta = 0`, si la presión ya venía alta *antes* de la primera
hora exportada en `percepciones.csv`, el agente basado en modelo no tiene
forma de saberlo y trata esa primera hora como si fuera la primera vez que
ocurre. Se incluyó porque es una limitación real del diseño (no un bug) que
afecta la interpretación de la bitácora, y el criterio de evaluación pide
explícitamente "discusión de limitaciones".
