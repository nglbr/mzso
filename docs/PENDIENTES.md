# PENDIENTES

Lo que sigue abierto. Nada de esto bloquea empezar la fase 0, salvo donde se indique.

---

## Verificaciones (checks rápidos)

- [ ] Granularidad de MacroFactor a Health Connect (20 min, en el teléfono). ¿Manda el detalle por alimento o solo el agregado diario? Define si nutricion_alimento y las alertas de despensa son posibles por esa vía, o si el screenshot por Telegram pasa a ser el plan principal. Bloquea la parte de despensa, no el resto.
- [ ] Garmin: ¿la cuenta tiene MFA y dónde queda el token? Se resuelve en el spike local de la fase 0.
- [ ] Garmin desde Cloud Run: ¿Garmin acepta logins desde IPs de Google? La librería no es oficial. Se prueba en la fase 3. Plan B: ingesta local.
- [ ] Garmin: ¿meter ejercicios en la app aporta algo? Solo si mejora la captura de series. Si no, la verdad del gym vive en Sheets.
- [ ] Niveles gratuitos vigentes de Cloud Run, Cloud Scheduler, Secret Manager, Artifact Registry y GCS, y en qué regiones aplican. Se verifica al crear la cuenta.

## Decisiones abiertas

- [ ] Región de GCP. Se decide después de verificar los niveles gratuitos.
- [ ] Artifact Registry: uno o dos repositorios para las dos imágenes.
- [ ] Persistencia y renovación del token de Garmin en Secret Manager (depende del spike).
- [ ] Cortar el gasto por programación (función que desactiva la facturación al pasar un umbral). Tarea posterior; mientras tanto, revisar el reporte de facturación las primeras semanas.
- [ ] Unidad del RAG científico (cuando lleguemos al agente). Opciones: claims curados, chunk-and-embed, híbrido, búsqueda en vivo. Recomendación previa: empezar con unos 20 claims curados, no 150. Marco: los papers dan el rango, los datos del usuario dicen dónde cae dentro del rango.
- [ ] Captura de percepciones: versión simple (campo estructurado junto a las series) o rica (chat en lenguaje natural). La simple no necesita IA.
- [ ] Extensión de precios entre tiendas. Aparcada: es otro problema.
- [ ] Acceso al dashboard desde el teléfono. Fuera de la rebanada uno; opciones a evaluar después: IAP o login propio en Streamlit.

## Definido (ya no bloquea)

- [x] Un solo usuario. Sin cuentas de usuarios.
- [x] Criterio de listo de la rebanada uno: 04_criterio_listo.md.
- [x] Stack con objetivo de aprendizaje: 03_stack.md (Cloud Run, Terraform, WIF, Podman, Secret Manager).
- [x] Dos imágenes: mzso-dashboard y mzso-ingest.
- [x] Dashboard: Streamlit en Cloud Run privado, abierto desde el PC con gcloud run services proxy.
- [x] Token de Garmin: no en Sheets. Va en Secret Manager.
- [x] Acceso a Sheets: cuenta de servicio adjunta, sin llave JSON.
- [x] Forma de trabajo y CLAUDE.md: 06_forma_de_trabajo.md.
- [x] Orden de construcción: 05_plan_construccion.md.

## Por escribir

- [ ] ADR 0001 a 0004 (ver 06_forma_de_trabajo.md), uno por uno, al cerrar la fase correspondiente.
- [ ] PROGRESS.md, cuando empiece la primera tarea.

## Fuentes verificadas (para cuando toque)

- python-garminconnect: vivo, v0.3.4 (mayo 2026), más de 130 métodos. Vía sólida, pero no oficial.
- API no oficial de MacroFactor: muerta (Firebase App Check, mayo 2026). No perseguir. Vías: Health Connect o export.
- RAG científico: OpenAlex (CC0), Europe PMC (full text OA), PubMed (filtro por meta-análisis), SportRxiv (sin peer review).
