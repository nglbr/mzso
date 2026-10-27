# MZSO

Sistema de seguimiento de gym y macros. Un solo usuario. Leer docs/README.md primero.

## Cómo trabajar

- Responder en español.
- Planear primero. No construir, instalar ni ejecutar nada sin una orden explícita del usuario. Elegir una opción autoriza su diseño, no su construcción.
- Un paso a la vez. Esperar el visto bueno antes del siguiente.
- Corregir al usuario cuando se equivoque. Nada de acuerdo automático.
- El usuario está aprendiendo: explicar qué se hace, cómo se hace y por qué.
- Cambios pequeños: una rama, una intención, un PR.

## Seguridad

- Solo cuentas y equipos personales. Ninguna información de terceros (repos, tickets, arquitectura).
- Secretos solo en Secret Manager. Nunca en el repo, el Dockerfile ni texto plano.
- No editar el state de Terraform a mano. plan es seguro, apply no se corre sin orden explícita.
- Despliega solo el pipeline.
- La app registra y muestra. No da prescripciones médicas ni nutricionales.

## Comandos

(se llenan cuando exista el Taskfile)

## Estructura

- docs/ fuente de verdad
- docs/adr/ decisiones
- PROGRESS.md estado
