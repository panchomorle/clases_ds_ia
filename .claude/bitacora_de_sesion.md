# Bitácora de la sesión

Cronología de lo que se hizo con el asistente, en orden. Incluye los
malentendidos que hubo, para que quede claro por qué el trabajo terminó
donde terminó.

1. **Clonación del repo.** Se clonó `panchomorle/clases_ds_ia` en la carpeta
   de trabajo.

2. **Primer malentendido sobre cuál era el TP.** El usuario pidió ayuda con
   `LABORATORIOS/Laboratorios_integradores.md`, creyendo que ese era el
   trabajo práctico a entregar. Ese documento en realidad describe **cuatro
   laboratorios integradores distintos** (Agua, Movilidad, Reclamos,
   Vegetales), cada uno con su propia fuente de datos — es un documento
   marco de la cátedra, no un TP puntual. Se le preguntó al usuario cuál de
   los cuatro correspondía a su grupo.

3. **Corrección del usuario.** El usuario aclaró que el archivo correcto
   era `consigna_agentes_movilidad.pdf`, dentro de `LABORATORIOS/MOVILIDAD/`
   — un TP completamente distinto, sobre agentes reactivos (IA clásica,
   Russell & Norvig), no sobre el pipeline de ciencia de datos del
   documento anterior.

4. **Lectura de la consigna real.** Se leyó
   `consigna_agentes_movilidad.md` y más adelante también el `.pdf`, para
   confirmar que ambos tienen el mismo contenido (se verificó texto por
   texto: coinciden).

5. **Explicación en criollo.** El usuario declaró no tener base en la
   materia. Se explicó el TP con la analogía del termostato antes de tocar
   código, y se corrigió un error propio: no hay una "empresa Y", sino
   "otras empresas" como categoría genérica.

6. **Implementación de `agentes_movilidad.py`.** Se completaron las 3
   partes pedidas: `decidir_reactivo_simple`, `actualizar_estado` +
   `decidir_reactivo_modelo`, y `procesar_secuencia`. Ver el detalle de cada
   decisión en `decisiones_de_diseno.md`.

7. **Creación de la rama de trabajo.** A pedido del usuario, se creó la
   rama `tp-agentes-movilidad` (partiendo de `main`) para no trabajar
   directamente sobre la rama principal.

8. **Tests obligatorios.** Se escribió `test_agentes_movilidad.py` con 8
   tests: los 3 casos obligatorios de la tabla de la consigna, la prueba
   decisiva de dos historias distintas, dos tests de validación de entradas
   no pedidos literalmente pero exigidos por el resto del enunciado, y dos
   tests de causalidad temporal (firma de funciones + comportamiento ante
   cambios futuros). Los 8 pasan.

9. **Primer commit.** `agentes_movilidad.py` + `test_agentes_movilidad.py`,
   a pedido explícito del usuario ("comitea TODOS los cambios").

10. **Instalación de dependencias pesadas.** Se instalaron `pyarrow` y
    `geopandas` para poder correr el simulador real (además de `pandas`,
    `numpy` y `pytest`, instalados antes).

11. **Bug de Windows.** Al correr
    `simulador_entorno_agente.py --zona 161 --hora 8 --taxis-x 20
    --horas-historia 3 --semilla 42`, falló la descarga del shapefile de
    zonas con `PermissionError`. Causa: `tempfile.NamedTemporaryFile` en
    Windows bloquea el archivo en modo exclusivo y `urlretrieve` no puede
    reabrirlo para escribir. Se corrigió en `simulador_movilidad.py`
    (función `cargar_centros_zonas`) usando `tempfile.mkstemp` con limpieza
    manual — ver justificación completa en `decisiones_de_diseno.md`.

12. **Generación del escenario reproducible.** Con el fix aplicado, el
    comando de ejemplo de la consigna corrió sin problemas y generó
    `escenario_agente/percepciones.csv` (zona 161, Midtown Center, horas 6
    a 8) y `escenario_agente/resultado_h_mas_1.csv` (hora 9, reservado).

13. **Generación de la bitácora.** Se escribió `generar_bitacora.py`, que
    lee `percepciones.csv` y corre `procesar_secuencia`, produciendo
    `bitacora_agentes.csv`. El resultado muestra la hora 6 como el caso
    donde los dos agentes difieren (simple recomienda, basado en modelo
    todavía no, racha=1) y las horas 7-8 donde coinciden (racha ≥ 2).

14. **Informe.** Se escribió `informe.md` con las 5 respuestas conceptuales
    (usando la evidencia real de la bitácora, no solo teoría genérica), la
    tabla PEAS instanciada con los datos concretos del escenario, y las 6
    limitaciones que exige la consigna más una encontrada durante el
    trabajo (efecto de borde del historial exportado).

15. **Guía de revisión.** Se escribió `CONSIGNAS_DE_REVISION.md`: checklist
    de 8 pasos para que el equipo (humanos) revise manualmente el 100% de
    la entrega contra la consigna y la rúbrica de evaluación, incluyendo
    una nota explícita sobre el fix de Windows para que no genere dudas al
    revisar el diff.

16. **Segundo commit.** Todo lo anterior (fix de `simulador_movilidad.py`,
    escenario, bitácora, informe, guía de revisión).

17. **Documentación del proyecto.** A pedido del usuario, se creó esta
    carpeta `.claude/` con toda la documentación de proceso, un
    `README.md` en la raíz del repo, y un `README.md` específico dentro de
    `LABORATORIOS/MOVILIDAD/`. También se agregó un `.gitignore` para
    `__pycache__/` y `.pytest_cache/`, que venían apareciendo como archivos
    sin trackear en cada `git status`.

## Pendiente para una próxima sesión

- Explicación línea por línea de `agentes_movilidad.py`, pedida por el
  usuario para "más adelante" (ver `estado_actual.md`).
- Revisión humana del equipo siguiendo `CONSIGNAS_DE_REVISION.md`.
- No se hizo `git push` a ningún remoto en ningún momento; todo el trabajo
  quedó commiteado en la rama local `tp-agentes-movilidad`.
