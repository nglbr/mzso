# ADR 0001: Google Sheets como almacén y AppSheet como captura

Estado: aceptada
Fecha: 2026-10-26

## Contexto

El sistema reúne datos de gym, nutrición, mediciones corporales y actividad para un solo usuario. Los volúmenes son pequeños (miles de filas por año). La captura desde el teléfono, sobre todo de las series de gym, es la interacción más frecuente y la más difícil de hacer cómoda.

## Decisión

Google Sheets es el almacén, con una pestaña por tabla, y AppSheet es la capa de captura móvil. El código accede a los datos a través de una capa de repositorio.

## Alternativas consideradas

- Firestore: se integra con IAM y con Terraform, pero obliga a construir la captura móvil desde cero.
- Supabase (Postgres): ofrece esquema y concurrencia reales, pero queda fuera de GCP y su plan gratuito puede pausarse por inactividad.
- SQLite local: no sirve en Cloud Run, cuyo sistema de archivos es efímero.

## Consecuencias

- A favor: captura móvil sin construir una app, datos visibles y editables a mano, ningún servidor de base de datos que operar.
- En contra: sin esquema forzado ni transacciones, cuotas en la API de Sheets y consultas menos expresivas.
- Mitigación: la capa de repositorio aísla el almacenamiento, así que cambiar de almacén toca un solo módulo.
