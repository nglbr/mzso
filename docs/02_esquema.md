# 02 — Esquema de datos

Vive en Google Sheets (una pestaña por tabla). La idea que gobierna todo es **el grano**: qué representa una fila. Elegido el grano, lo que puedas calcular después queda determinado.

Las tablas de **config** y **captura** las controla el usuario y son estables. Las de **ingesta** dependen del JSON/export real de cada app, así que se bocetan aquí y se finalizan contra el payload real al construir el conector.

---

## Config (casi nunca cambia)

### `objetivo` — grano: un periodo
`fecha_inicio` · `fecha_fin` · `objetivo` · `prioridad` · `notas`
Hace que el éxito se juzgue por bloque.

### `catalogo_ejercicios` — grano: un ejercicio (canónico)
`ejercicio_id` · `ejercicio` · `patron` · `musculo_primario` · `musculo_secundario` · `peso_primario` · `peso_secundario` · `equipo` · `activo`

- **`ejercicio_id`**: clave estable (slug o número). El resto de tablas referencian este id, no el texto → renombrar un ejercicio no rompe el histórico.
- **Canónico**: el mismo ejercicio es el mismo trabaje el gym que sea. El **gimnasio y la máquina específica NO van aquí** — van en `series` (dónde realmente se hizo). Así, al cambiar de gym, la similitud sale del mapeo de músculo y el historial de dónde sale de las series.
- **`activo`** (booleano): marca si está en la rotación actual. Pragmático; suficiente.
- **La tabla que hace o rompe el proyecto:** sin un mapeo bueno de ejercicio → grupo muscular con pesos, "volumen por grupo muscular" es basura. ~40–70 ejercicios, curados a mano una vez.

---

## Captura (manual por AppSheet)

### `series` — grano: **una serie**
`fecha` · `sesion_id` · `dia_tipo` · `ejercicio_id` · `serie_num` · `peso_kg` · `reps` · `rir` · `parciales` · `gimnasio` · `nota`

- Grano = la serie (confirmado). De aquí salen e1RM, progresión por ejercicio y series efectivas. Guardar por sesión perdería todo eso.
- **`sesion_id`** (= `fecha` + `dia_tipo`) agrupa: modelo en tres niveles **sesión → ejercicio → serie**, todo unible con JOIN.
- Clave única de una serie: `fecha` + `ejercicio_id` + `serie_num`.
- **`rir`**: se empieza a registrar → habilita *series efectivas* (la métrica buena de volumen).
- En Sheets los ids pueden ser columnas calculadas con fórmula.

### `peso_composicion` — grano: una medición matutina
`fecha` · `hora` · `peso_kg` · `bmi` · `grasa_pct` · `musculo_esqueletico_pct` · `rm_kcal` · `body_age` · `nota`
(Orden de columnas = orden de captura de la báscula.)

### `medidas` — grano: una medición (semanal), formato **largo**
`fecha` · `parte` · `cm`
Largo para agregar partes nuevas sin cambiar la estructura.

### `percepciones` — grano: una entrada
`fecha` · `contexto` (día/sesión/ejercicio) · `rpe_sesion` · `dolor_grupo` · `sueno_percibido` · `motivacion` · `hambre` · `nota_libre`

### `compras` — grano: una compra (solo indispensables)
`fecha` · `producto` · `cantidad_g` · `precio` · `tienda`
Alimenta el inventario inferido. Puede esperar un poco.

---

## Ingesta (cae solo desde las apps — columnas a finalizar contra el payload real)

### `actividades` (Garmin) — grano: una actividad
`fecha` · `tipo` · `duracion_min` · `calorias` · `fc_media` · `fc_max` · `nota`

### `salud_diaria` (Garmin) — grano: un día
`fecha` · `hrv` · `fc_reposo` · `sueno_score` · `sueno_horas` · `body_battery` · `estres`

### `nutricion_diaria` (MacroFactor) — grano: un día
`fecha` · `kcal` · `prote_g` · `carbs_g` · `grasa_g` · `fibra_g` · `expenditure` · `peso_tendencia_mf` · `adherencia` · `target_kcal` · `target_prote` · `target_carbs` · `target_grasa`

### `nutricion_alimento` (MacroFactor) — grano: un alimento loggeado *(condicional)*
`fecha` · `comida` · `alimento` · `cantidad_g` · `kcal` · `prote` · `carbs` · `grasa`
**Solo existe si el check de granularidad sale bien.** Es la que habilita las alertas de despensa.

---

## Núcleo de la rebanada uno

Siete tablas bastan para "todo unido" + la vista esperado vs. real:
`objetivo` · `catalogo_ejercicios` · `series` · `peso_composicion` · `actividades` · `salud_diaria` · `nutricion_diaria`.

`medidas`, `percepciones`, `nutricion_alimento` y `compras` se cuelgan enseguida.
