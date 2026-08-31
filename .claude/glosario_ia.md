# Glosario de IA para este TP

Escrito para alguien que arranca sin conocimientos previos de la materia
(fue el caso real durante esta sesión). Todo se explica con el ejemplo
concreto del TP de taxis.

## Agente

Un programa (o entidad) que **percibe** información del entorno y **decide**
una **acción** en base a ella. No hace falta que sea inteligencia artificial
compleja: un termostato que prende la calefacción cuando hace frío ya es un
agente. En este TP, el agente percibe la "presión" de demanda de taxis en
una zona y decide si recomendar reforzar la flota.

## Percepción (`percepcion`)

La información que el agente recibe en un instante dado. En este TP es una
fila de `percepciones.csv`: zona, hora, cuántos taxis tiene la empresa,
cuánta demanda hubo, la presión resultante, etc.

## Acción

Lo que el agente decide hacer. Acá son mensajes, no ejecuciones reales:
`NO_REFORZAR`, `RECOMENDAR_REFUERZO`, `ABSTENERSE`.

## Agente reactivo simple

Decide **solo con la percepción actual**, sin memoria de lo que pasó antes.
Regla condición → acción directa. Ejemplo del TP: "si la presión de esta
hora es ≥ 0.85, recomendar refuerzo". No importa qué pasó las horas
anteriores.

## Agente reactivo basado en modelo

También decide con una regla condición → acción, pero antes actualiza un
**estado interno** (una memoria resumida) que combina la percepción actual
con el estado anterior. Sigue siendo "reactivo" —no planifica ni busca
caminos hacia un objetivo— pero su decisión depende de la *historia*
reciente, no solo del instante presente. Ejemplo del TP: cuenta cuántas
horas seguidas la presión estuvo alta (`racha_presion_alta`) y solo
recomienda refuerzo si son 2 o más horas seguidas.

**Diferencia clave con un agente planificador:** un agente basado en modelo
no genera "sucesores" ni evalúa secuencias futuras de acciones; solo resume
el pasado para interpretar mejor el presente. Planificar sería, por
ejemplo, simular distintas asignaciones de taxis a futuro y elegir la
mejor secuencia — eso NO es parte de este TP.

## Estado interno

La memoria que carga el agente basado en modelo entre una decisión y la
siguiente. En este TP tiene 4 campos: si la última percepción fue válida,
cuántas horas seguidas hubo presión alta, cuál fue la presión anterior, y
cuál fue la última acción tomada.

## PEAS

Framework para describir un agente en 4 partes:

- **P**erformance (rendimiento): qué se considera una buena decisión.
- **E**nvironment (entorno): en qué mundo opera el agente.
- **A**ctuators (actuadores): cómo actúa sobre el mundo.
- **S**ensors (sensores): cómo percibe el mundo.

Ver la tabla completa aplicada a este TP en
`LABORATORIOS/MOVILIDAD/informe.md`.

## Causalidad temporal / fuga de datos (data leakage)

Un agente que decide en el instante `h` **no puede** usar información que
solo existe después de `h` (por ejemplo, la hora `h+1`). Si la usara,
estaría "haciendo trampa": en la vida real esa información todavía no
existiría al momento de decidir. Por eso `resultado_h_mas_1.csv` está
separado y nunca se lee dentro de las funciones de decisión — usarlo
invalidaría por completo la comparación entre agentes.

## Umbral y racha

- **Umbral** (`UMBRAL_PRESION = 0.85`): el valor de presión a partir del
  cual se considera que hay evidencia de estrés en la zona.
- **Racha** (`racha_presion_alta`): cuántas observaciones consecutivas
  estuvieron por encima del umbral. Se resetea a 0 apenas la presión baja
  del umbral (o si la percepción es inválida).

## Percepción inválida / abstención

Si faltan datos, hay valores no numéricos (`NaN`, infinito) o la capacidad
de la empresa es 0 o desconocida, el agente no tiene base para decidir con
seguridad y responde `ABSTENERSE` en vez de arriesgar una recomendación
sin sustento.
