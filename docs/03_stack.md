# 03 Stack (rebanada uno)

MZSO tiene dos objetivos. El del producto: unir los datos de gym y nutrición del usuario y que muestren algo que antes no se veía. El de aprendizaje: practicar contenedores, GCP, Terraform, CI/CD y Git. Cuando chocan, gana el de aprendizaje, siempre que el criterio de listo (04) se siga cumpliendo.

Todo con cuentas y equipo personales (GitHub, proyecto de GCP). Ninguna información de terceros.

## Decisiones

| Pieza | Elección | Por qué |
|---|---|---|
| Almacén | Google Sheets | Es el punto de encuentro de todas las piezas. Con un usuario sobra. Se accede a través de una capa de repositorio para poder cambiarlo tocando un solo archivo. |
| Captura | AppSheet | App móvil sobre Sheets sin construir nada. La captura de gym es la UX más difícil y no es lo que quiero aprender. |
| Código y flujo | GitHub personal, ramas y pull requests siempre | Practicar el flujo real aunque trabaje solo. Rama protegida: nada entra a main sin pasar el CI. |
| Contenedores | Dos imágenes: mzso-dashboard y mzso-ingest, cada una con su Dockerfile, construidas con Podman | Separa dependencias (Streamlit pesa, la ingesta no) y da más práctica. Si Podman falla, revisar primero su entorno de ejecución (WSL2 o máquina virtual). |
| Registro de imágenes | Artifact Registry, inspección con Crane | Es donde Cloud Run lee las imágenes. Crane permite ver capas y etiquetas sin un demonio. |
| Ingesta y cerebro | Cloud Run Job disparado por Cloud Scheduler | Corre, termina y no cobra mientras no corre. Sustituye al cron de Actions y permite guardar el token de Garmin en Secret Manager. |
| Dashboard | Streamlit en Cloud Run, privado | Sin URL pública. Se abre desde el PC con gcloud run services proxy, que firma las peticiones con la identidad del usuario. El teléfono queda para después. |
| Infraestructura | Terraform, state remoto en un bucket de GCS con bloqueo | Todo lo de arriba queda declarado y repetible. El state nunca se edita a mano. plan es seguro, apply ejecuta cambios. |
| CI/CD | GitHub Actions con Workload Identity Federation | Sin llaves JSON descargadas. El despliegue ocurre solo desde el pipeline. |
| Secretos | Secret Manager | Nunca en el repo, el Dockerfile ni variables en texto plano. |
| Acceso a Sheets | Cuenta de servicio adjunta al servicio, sin llave | Se comparte el Sheet con el email de esa cuenta. No hay credenciales que guardar. |
| Comandos | Taskfile.yml (build, test, deploy) | Un solo lugar para los comandos, igual para el usuario que para el pipeline. |

## Forma de la rebanada uno

```
AppSheet ──► escribe a Google Sheets (memoria)
Cloud Scheduler ──► dispara el Cloud Run Job (mzso-ingest):
     ├─ ingesta: Garmin + export MacroFactor ──► Sheets
     └─ cerebro: lee Sheets, calcula, escribe pestañas "gold"
Streamlit (mzso-dashboard, Cloud Run privado) ──► lee las "gold"
```

## El seam clave

Captura, cerebro y dashboard no se hablan directo. Se encuentran en Sheets. Python deja resultados en pestañas "gold" y el dashboard solo las lee. Cada pieza se cambia sin tocar las otras. El costo es que el dashboard muestra lo último calculado, no el segundo exacto, y eso no importa porque la data no es de tiempo real.

## Riesgos

1. Garmin puede rechazar logins desde IPs de Google, porque la librería no es oficial. El spike debe probarse desde Cloud Run, no solo desde la máquina local. Plan B: correr la ingesta localmente.
2. Bootstrap. El bucket del state y la configuración de WIF no los puede crear el mismo Terraform que los necesita. Se crean una vez a mano y se documentan.
3. Costo. La alerta de presupuesto solo avisa, no corta el gasto. Ponerla desde el primer día, revisar el reporte de facturación las primeras semanas y verificar los límites gratuitos vigentes de cada servicio antes de contar con ellos. Cortar el gasto por programación queda como tarea posterior.
4. Sobreingeniería para un usuario. Es deliberada, es el punto de aprendizaje.
5. La captura de gym es la pantalla más usada. Ahí se gana o se pierde la adherencia.

## Decisiones abiertas

- Región de GCP. Se decide al crear la cuenta, después de verificar en las páginas de precios dónde aplica el nivel gratuito de cada servicio.
- Si los repositorios de Artifact Registry son uno o dos.
- Cómo persistir y renovar el token de Garmin en Secret Manager (depende del spike de Garmin y de si la cuenta tiene MFA).

## Fuera de alcance por ahora

Agente conversacional, RAG científico, alertas proactivas, despensa, RabbitMQ, Kubernetes, dos entornos (dev y prod), Next.js, acceso desde el teléfono. Todo se cuelga encima de la base cuando ya camine.
