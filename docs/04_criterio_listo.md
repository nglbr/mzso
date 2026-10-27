# 04 Criterio de listo (rebanada uno)

Un buen criterio es observable y binario (pasa o no pasa), se escribe desde el lado del usuario ("se puede ver X") e incluye una barra de valor, no solo funcional. Un sistema que corre pero no sirve no cumple.

## Alcance de la rebanada uno: ultra-delgada

Unir, hacer visible y tener números confiables: todos los datos juntos, en tendencias, en una pantalla.

El "esperado vs. real + porqué" (atribución que cruza fuentes) es la rebanada dos, montada sobre la misma plomería.

---

## A. El hilo camina de punta a punta
- [ ] Se registra una sesión de gym completa desde AppSheet en el teléfono y aparece limpia en Sheets, sin escribir notación a mano.
- [ ] Se registran peso y composición corporal de la mañana en menos de ~20 segundos.
- [ ] El job de Python obtiene los últimos días de Garmin (actividad y salud) y los escribe a Sheets, corriendo solo en la nube y sin depender del equipo local.
- [ ] El export histórico de MacroFactor está cargado en Sheets.
- [ ] El dashboard de Streamlit abre y muestra en una pantalla: tendencia de peso, tendencia de grasa y músculo, e1RM por levantamiento principal, volumen semanal por grupo muscular y actividades (incluidas las ajenas al gym).

## B. Los números son confiables (verificación, innegociable)
- [ ] Para un ejercicio y una semana, e1RM y volumen efectivo calculados a mano coinciden con los del sistema.
- [ ] La tendencia de peso usa media móvil, no el dato crudo, y se ve la diferencia.
- [ ] Un dato basura (sync duplicado de Garmin, un peso mal tecleado) no rompe el dashboard ni ensucia la tendencia en silencio.

## C. La barra de valor (la que de verdad importa)
- [ ] El dashboard muestra al menos una cosa que antes no se sabía o no se tenía junta.
- [ ] Se usa una vez para tomar o informar una decisión real de la semana.

---

## Definición de "listo" en una frase

> Los datos se registran sin fricción, los de Garmin y MacroFactor entran solos, y en una pantalla se ve todo unido y en tendencias confiables, y ya mostró algo que antes no se veía.
