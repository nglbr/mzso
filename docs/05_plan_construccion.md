# 05 Plan de construcción (rebanada uno)

El cómo. Cada tarea empuja hacia un criterio de 04_criterio_listo.md. El orden no es capricho: los prerrequisitos y spikes primero (pueden tumbar supuestos), luego un esqueleto de infraestructura con un "hola mundo" (para detectar errores de infraestructura antes de tocar la lógica de negocio), y la vista al final porque se para sobre todo lo demás.

Cada tarea sigue el ciclo de 06_forma_de_trabajo.md: rama, cambio pequeño, pull request, CI, mezclar. Al cerrar cada fase se actualiza PROGRESS.md y, si cambió una decisión grande, el ADR correspondiente.

---

## Fase 0 Prerrequisitos y spikes

- [ ] Cuenta de GCP personal, proyecto nuevo y alerta de presupuesto desde el primer día. La alerta avisa, no corta el gasto.
- [ ] Verificar en las páginas de precios el nivel gratuito de cada servicio y decidir la región.
- [ ] Herramientas en el equipo local: Git, gcloud, Terraform, Podman, Task, Crane, Python.
- [ ] Repo en GitHub personal, rama main protegida (nada entra sin PR y CI).
- [ ] Spike Health Connect: registrar una comida en MacroFactor y ver si llega por alimento o solo el total diario. Decide si la despensa va por Health Connect o por screenshot.
- [ ] Spike Garmin local: login con python-garminconnect, confirmar que jala, ver si la cuenta tiene MFA y localizar el token.
- [ ] Spike export de MacroFactor: un export manual y mirar las columnas reales para cerrar nutricion_diaria.

## Fase 1 Esqueleto andante

- [ ] Bootstrap manual y documentado: bucket de GCS para el state de Terraform, pool y proveedor de WIF, cuenta de servicio del pipeline.
- [ ] Terraform: activar APIs, Artifact Registry, Cloud Run con un servicio "hola mundo" privado.
- [ ] Dockerfile mínimo, construido con Podman, subido a Artifact Registry, inspeccionado con Crane.
- [ ] Pipeline de GitHub Actions: en cada PR corre pruebas, terraform fmt, validate y plan. Al mezclar a main construye, sube la imagen y aplica.
- [ ] Taskfile con build, test y deploy.
- [ ] Verificar: un cambio trivial entra por PR y llega a Cloud Run sin tocar la consola, y el servicio se abre con gcloud run services proxy.
- [ ] Escribir los ADR 0002, 0003 y 0004.

## Fase 2 Cimientos de datos y captura

Puede avanzar en paralelo con la fase 1, y conviene empezarla pronto: así los datos de gym se acumulan mientras se construye la cadena.

- [ ] Crear el Google Sheet con las 7 pestañas núcleo y encabezados (del esquema).
- [ ] Curar catalogo_ejercicios a mano (40 a 70 ejercicios con mapeo de músculo). La tabla que hace o rompe todo.
- [ ] AppSheet sobre el Sheet: formularios para series (la UX difícil), peso_composicion y medidas.
- [ ] Prueba en el teléfono: meter una sesión real y verificar filas limpias. Criterios A1 y A2.
- [ ] Escribir el ADR 0001.

## Fase 3 Ingesta

- [ ] Imagen mzso-ingest con su Dockerfile y el paquete compartido de acceso a Sheets (capa de repositorio).
- [ ] Cuenta de servicio del job con acceso al Sheet (compartido con su email, sin llave).
- [ ] Cloud Run Job y Cloud Scheduler, definidos en Terraform.
- [ ] Token de Garmin en Secret Manager, con el job escribiendo una versión nueva al renovarlo.
- [ ] Spike Garmin desde Cloud Run: confirmar que el login funciona desde IPs de Google. Si falla, plan B: ingesta local.
- [ ] Cargar el export histórico de MacroFactor a nutricion_diaria. Criterios A3 y A4.

## Fase 4 Cerebro

- [ ] Pruebas primero, luego las funciones: e1RM por ejercicio, volumen efectivo semanal por grupo muscular (JOIN series con catalogo_ejercicios), tendencia de peso con media móvil y tendencias de la báscula.
- [ ] Escribir resultados a las pestañas gold.
- [ ] Verificar a mano un ejercicio y una semana. Criterio B.

## Fase 5 Vista

- [ ] Imagen mzso-dashboard con Streamlit, desplegada en Cloud Run privado por el pipeline.
- [ ] Pantalla única con las 5 vistas, leyendo solo las gold, abierta con gcloud run services proxy. Criterio A5.

## Fase 6 Cierre de valor

- [ ] Verificar que muestra algo que antes no se veía y usarlo una vez para una decisión real. Criterio C.

---

Las fases 1 y 2 avanzan en paralelo. La fase 4 necesita datos en Sheets (fases 2 y 3). Nada de la fase 5 empieza antes de que la 4 pase su verificación a mano.
