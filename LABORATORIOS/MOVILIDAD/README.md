# TP: Agentes reactivos para refuerzo de taxis

Trabajo práctico de IA clásica (no Machine Learning): programar y comparar
un agente reactivo simple y un agente reactivo basado en modelo que
recomiendan si conviene reforzar la flota de taxis de una empresa ficticia
"X" en una zona, para la hora siguiente. Enunciado completo en
[`consigna_agentes_movilidad.md`](consigna_agentes_movilidad.md) (idéntico a
`consigna_agentes_movilidad.pdf`, verificado).

Si no tenés base en la materia, empezá por
[`../../.claude/glosario_ia.md`](../../.claude/glosario_ia.md).

## Mapa de archivos

| Archivo | Qué es |
|---|---|
| `consigna_agentes_movilidad.md` / `.pdf` | Enunciado original del TP (misma información en los dos formatos). |
| `plantilla_agentes_movilidad.py` | Plantilla original de la cátedra, sin completar. Se conserva como referencia. |
| **`agentes_movilidad.py`** | **Entrega 1.** Los dos agentes implementados: `decidir_reactivo_simple`, `crear_estado_inicial` / `actualizar_estado` / `decidir_reactivo_modelo`, y `procesar_secuencia`. |
| **`test_agentes_movilidad.py`** | **Entrega 2.** Los tests obligatorios (8 en total). Correr con `python -m pytest test_agentes_movilidad.py -v`. |
| `simulador_movilidad.py` | Simulador de viajes de taxi (provisto por la cátedra). Se corrigió un bug de compatibilidad con Windows — ver `../../.claude/decisiones_de_diseno.md`. |
| `simulador_entorno_agente.py` | Transforma los viajes simulados en percepciones horarias para los agentes. |
| `escenario_agente/percepciones.csv` | Escenario reproducible ya generado (zona 161, hora 8, 20 taxis de X, semilla 42). Entrada permitida para los agentes. |
| `escenario_agente/resultado_h_mas_1.csv` | Resultado de la hora `h+1`. **Reservado para evaluación** — nunca se usa para decidir. |
| `generar_bitacora.py` | Script que lee `percepciones.csv` y corre `procesar_secuencia` para producir la bitácora. |
| **`bitacora_agentes.csv`** | **Entrega 3.** Comparación de ambos agentes, hora por hora, sobre el escenario reproducible. |
| **`informe.md`** | **Entrega 4.** Las 5 respuestas conceptuales, el PEAS y las limitaciones que pide la consigna. |
| `CONSIGNAS_DE_REVISION.md` | Checklist paso a paso para que el equipo revise el 100% de la entrega antes de mandarla. |
| `requirements.txt` | Dependencias de Python necesarias. |
| `dataset_movilidad.py`, `DataSet_Movilidad.ipynb` / `.pdf`, `README_simulador_movilidad.md` | Material general de la cátedra sobre el dataset TLC y el simulador. No son parte puntual de esta entrega, pero explican cómo funciona `simulador_movilidad.py` por dentro. |

## Cómo correr todo de nuevo

Desde esta carpeta (`LABORATORIOS/MOVILIDAD/`):

```bash
# Instalar dependencias (desde la raíz del repo también funciona)
python -m pip install -r requirements.txt

# Correr los tests obligatorios
python -m pytest test_agentes_movilidad.py -v

# Regenerar el escenario reproducible (necesita conexión a internet:
# descarga datos reales de TLC y el shapefile de zonas)
python simulador_entorno_agente.py --zona 161 --hora 8 --taxis-x 20 \
  --horas-historia 3 --semilla 42 --salida-dir escenario_agente

# Regenerar la bitácora a partir del escenario
python generar_bitacora.py
```

Con la misma semilla (`42`) y los mismos parámetros, el escenario generado
es siempre idéntico — así se verifica la reproducibilidad.

## Documentación de proceso

El detalle de por qué cada cosa quedó como quedó (decisiones de diseño,
cronología de la sesión, qué falta) está en [`../../.claude/`](../../.claude/README.md).
