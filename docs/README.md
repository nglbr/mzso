# MZSO

Sistema para decidir cada semana qué ajustar en entrenamiento y nutrición a partir de datos propios, reunidos en un solo lugar, en vez de seguir un plan genérico que no mide ni se corrige.

Un solo usuario. Construido para uso personal, no para un mercado.

---

## El problema

Los datos existen (rutinas en hojas de cálculo, nutrición en una app de macros, mediciones corporales en notas, actividad en un reloj deportivo) pero están dispersos y desconectados. El problema no es registrar, es unir: que la relación entre fuentes, por ejemplo entre nutrición y rendimiento, no dependa de notarla por accidente y tarde.

## El éxito

Que los objetivos se cumplan. Como el objetivo cambia en el tiempo (por ejemplo, de hipertrofia a fuerza), el éxito se juzga por bloque: dentro de cada periodo, las métricas relevantes para ese objetivo se movieron bien. Al cambiar de objetivo, el bloque nuevo parte sustentado en el estado anterior y no en blanco.

## Principios

El sistema no juzga ni ordena. Da visibilidad y el porqué, y la decisión es del usuario.

- Observa y registra sin juzgar. El significado aparece en el conjunto: una actividad deportiva externa al gym cuenta como carga, una sustitución de comida se anota y no se regaña.
- Casi todo lo que produce tiene la misma forma: esperado vs. real, con el porqué desglosado, cruzando fuentes.
- Las sugerencias son opciones, no órdenes ("una opción sería...", nunca "hay que...").
- Indica cuándo no vale la pena optimizar algo, en vez de inventar precisión falsa.

## Para quién

Se asume un usuario con conocimiento de entrenamiento. El sistema optimiza, no enseña.

- Gym: el usuario diseña la rutina; el sistema observa y sugiere ajustes puntuales, sin imponer plantillas.
- Nutrición: ayuda activa.

Extenderlo a otros usuarios queda fuera de alcance por ahora.

---

## Los documentos

| Archivo | Para qué |
|---|---|
| `01_requisitos.md` | Qué debe hacer: los tres modos, las escenas y las fuentes de datos |
| `02_esquema.md` | El modelo de datos (pestañas de Sheets, grano, columnas, ids) |
| `03_stack.md` | Con qué se construye la rebanada uno y por qué |
| `04_criterio_listo.md` | Cómo se sabe que la rebanada uno está terminada |
| `05_plan_construccion.md` | Las tareas técnicas y en qué orden |
| `06_forma_de_trabajo.md` | Cómo se trabaja: ciclo de cada tarea, documentación mínima, ADR |
| `PENDIENTES.md` | Decisiones y verificaciones todavía abiertas |
