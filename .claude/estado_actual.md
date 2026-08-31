# Estado actual del TP

Última actualización: sesión del 2026-08-31. El PR
[#2](https://github.com/panchomorle/clases_ds_ia/pull/2) (rama
`tp-agentes-movilidad`) ya fue revisado, mergeado a `main` por el equipo y
la rama remota fue borrada. Este repositorio local ya está sincronizado:
`main` actualizado, rama local `tp-agentes-movilidad` eliminada, referencia
remota obsoleta podada (`git remote prune origin`).

## Terminado

- [x] `agentes_movilidad.py` — Parte 1, 2 y 3 completas.
- [x] `test_agentes_movilidad.py` — 8/8 tests pasan (`python -m pytest
      test_agentes_movilidad.py -v`).
- [x] Escenario reproducible generado (`escenario_agente/percepciones.csv`
      y `resultado_h_mas_1.csv`).
- [x] `bitacora_agentes.csv` generado con `generar_bitacora.py`.
- [x] `informe.md` — 5 respuestas, PEAS y limitaciones.
- [x] `CONSIGNAS_DE_REVISION.md` — checklist de revisión para el equipo.
- [x] Fix de compatibilidad Windows en `simulador_movilidad.py`,
      documentado en `decisiones_de_diseno.md` y en la propia guía de
      revisión.
- [x] Documentación de proceso (`.claude/`) y README en la raíz del repo y
      en `LABORATORIOS/MOVILIDAD/`.
- [x] Los 4 archivos que pide la sección "Entrega" de la consigna están
      presentes y verificados contra el enunciado.

## Pendiente

- [ ] **Explicación línea por línea de `agentes_movilidad.py`** para el
      usuario — quedó pendiente explícitamente para una próxima
      interacción.
- [ ] **Defensa individual** (sección 8 de `CONSIGNAS_DE_REVISION.md`):
      cada integrante debe poder explicar el TP sin mirar el código.
- [ ] Confirmar con la cátedra el criterio de entrega final (¿alcanza con
      que esté en `main` del fork del equipo, o hay que entregarlo por otro
      medio, por ejemplo un classroom o un link puntual?).

## Cómo retomar

1. Leer `bitacora_de_sesion.md` para el contexto completo del proceso.
2. Leer `decisiones_de_diseno.md` si hace falta justificar alguna decisión
   puntual del código.
3. Correr `python -m pytest LABORATORIOS/MOVILIDAD/test_agentes_movilidad.py -v`
   para confirmar que todo sigue en verde antes de seguir.
