# Estado actual del TP

Última actualización: sesión del 2026-08-31. Rama: `tp-agentes-movilidad`
(no fusionada a `main`, no pusheada a ningún remoto).

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
- [ ] **Revisión humana** del equipo siguiendo `CONSIGNAS_DE_REVISION.md`
      (los 8 pasos, incluyendo la sección de defensa individual).
- [ ] Decidir si se necesita hacer `push` a algún remoto (fork del equipo,
      classroom, etc.) — no se hizo ninguno todavía.
- [ ] Decidir si se fusiona `tp-agentes-movilidad` a `main` o se entrega
      directamente desde la rama.

## Cómo retomar

1. Leer `bitacora_de_sesion.md` para el contexto completo del proceso.
2. Leer `decisiones_de_diseno.md` si hace falta justificar alguna decisión
   puntual del código.
3. Correr `python -m pytest LABORATORIOS/MOVILIDAD/test_agentes_movilidad.py -v`
   para confirmar que todo sigue en verde antes de seguir.
