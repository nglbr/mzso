# 06 Forma de trabajo

Cómo se construye MZSO, con ayuda de un agente. Lo importante no es el tamaño del prompt: es definir antes qué significa terminado y verificar después con algo que no dependa de que el agente diga que funcionó.

## Ciclo de cada tarea

1. Planear: se discute el diseño antes de tocar nada.
2. Definir el criterio de listo de la tarea, con una prueba cuando se pueda.
3. Rama nueva, una intención por cambio.
4. Implementar y correr las pruebas.
5. Leer el diff completo. No se mezcla nada que no pueda explicar en voz alta.
6. Pull request pequeño. El CI valida. Mezclar.
7. Si cambió una decisión grande, actualizar el ADR. Anotar en PROGRESS.md.
8. Sesión nueva para la siguiente tarea.

## Documentación mínima

- CLAUDE.md: reglas de trabajo y comandos. Corto.
- docs/: requisitos, esquema, stack, criterio, plan, pendientes.
- docs/adr/: una decisión grande por archivo (contexto, decisión, consecuencias).
- PROGRESS.md: qué se hizo y qué sigue, cinco líneas por sesión.

No se crean glosario, deuda ni dudas aparte. PENDIENTES.md cumple esa función.

## ADR iniciales

- 0001 Sheets y AppSheet como almacén y captura
- 0002 Cloud Run Job en lugar de cron de Actions
- 0003 Dashboard privado con gcloud run services proxy
- 0004 WIF en lugar de llaves JSON

## Lo que no se delega al agente

Credenciales de producción, el state de Terraform y el contenido de los secretos.

## Regla sobre prompts

Un buen prompt lleva la intención, las restricciones y el criterio de listo. Ejemplo: en la rama feat/garmin-ingest, escribe fetch_daily_health(date) que devuelva las columnas de salud_diaria, primero la prueba con una respuesta simulada, sin tocar Terraform, y al terminar explica qué decidiste sobre los reintentos.
