# 01 Requisitos

Qué debe hacer el sistema. Derivado de escenas de uso reales.

---

## Los tres modos

- Reactivo: el usuario pregunta en el momento.
- Reflexivo: el usuario se sienta a revisar.
- Proactivo: el sistema avisa por iniciativa propia.

## Las escenas

### Reactivas
- Sustitución de comida: falta un ingrediente. ¿Qué armar con el inventario disponible que cumpla el objetivo del día? El resultado también anota la desviación (esto por esto) para la revisión.
- Equipo ocupado: un ejercicio no está disponible. ¿Qué hacer en su lugar? Usa el mapeo de músculo del catálogo.
- Técnica: el usuario siente raro un movimiento y pide una explicación detallada a partir de su descripción y sus percepciones. Aquí el sistema no usa los datos del usuario, se apoya en documentación. Es la única escena sin ventaja de datos propios.
- Pregunta suelta: "¿voy bien de proteína?", "¿mi sentadilla sube o es impresión?". Responde con los números del usuario, no con generalidades.

### Reflexivas
- Diaria: pesaje y medición en la báscula de bioimpedancia, que entra sin fricción.
- Semanal: medidas de cinta. ¿El bloque va como debe según el objetivo activo?
- Mensual: toda la data, más percepciones, más lo que se preguntó, cambió o implementó, para sacar conclusiones e iterar.
- Estancamiento: 3 semanas sin mover fuerza ni medidas. El porqué desglosado, más opciones.
- Cambio de objetivo (por ejemplo, hipertrofia a fuerza): el bloque nuevo arranca con todo lo aprendido del bloque anterior.

### Proactivas
- Alerta de carbohidratos pre-entreno: aviso antes de la comida pre-entreno, con el timing optimizado según la fuente de carbohidrato y el horario. Debe decir cuándo la diferencia de timing no importa y lo que manda son los carbohidratos totales.
- Alerta de despensa: infiere el consumo desde MacroFactor contra lo comprado y avisa cuando algo se va a acabar. Depende del check de granularidad (ver PENDIENTES).

### Transversal (captura)
- Log de percepciones en cada capa: por ejercicio, por sesión, por día. Dureza (RPE), dolor por grupo, sueño percibido vs. medido, motivación, hambre.

---

## Principio de nutrición

No es micro-optimizar cada comida. Es un plan estable (misma dieta casi siempre, macros diarios como objetivo), más una optimización ligera del timing intradía (carbohidratos antes, proteína después, fibra), más registrar la realidad sin juzgar, incluidas salidas y sustituciones, para que el valor salga del agregado.

## Asimetría de intervención

El sistema interviene más en nutrición (ayuda activa) que en gym (solo observa).

---

## Las fuentes de datos

| Fuente | Qué aporta | Cómo entra |
|---|---|---|
| Google Sheets (historial de gym) | Rutinas, series y reps, notas ("mala técnica", "parciales" son señal, no ruido) | Captura manual (se re-estructura, ver esquema) |
| MacroFactor | Nutrición: macros diarios y, si se puede, por alimento | Health Connect (Android) o screenshot por Telegram, más export histórico |
| Notas del teléfono a Sheets | Peso y composición corporal diarios | Captura manual estructurada nueva |
| Garmin | Actividad (deporte, cardio) y salud (HRV, sueño, body battery) | `python-garminconnect` |
| Cinta métrica | Medidas por parte, semanal | Captura manual |
| Tab de despensa | Compras de indispensables (con precio) | Captura manual, solo indispensables |
| Log de percepciones | Subjetivo por ejercicio, sesión y día | Captura (chat o inline) |
| Objetivo con fecha | Qué se persigue en cada periodo | Lo declara el usuario |

Regla de oro: nunca se usa el número absoluto de peso ni de la báscula, siempre la tendencia, para que el error constante del instrumento se cancele al mirar la pendiente.
