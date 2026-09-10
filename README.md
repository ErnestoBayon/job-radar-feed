# Job Radar — feed público de vacantes

Feed generado automáticamente por el Job Radar (recolección diaria vía APIs públicas de Greenhouse, Ashby, Lever, Workday y Eightfold). Pensado para ser consumido por el pipeline de n8n ("Radar de Vacantes v4.5") como fuente #5.

## URL estable

```
https://raw.githubusercontent.com/ErnestoBayon/job-radar-feed/main/jobs_feed.json
```

- Se actualiza automáticamente cada día después del escaneo diario (~13:20 UTC), antes del pull de n8n a las 14:00 UTC.
- Contiene únicamente vacantes abiertas publicadas en las últimas 48 horas, capadas a 500, ordenadas por fecha de publicación descendente.
- Si no hay vacantes nuevas en la ventana, el archivo es un arreglo vacío `[]` (nunca un error).
- Metadatos de la última generación (hora, conteo) en [`meta.json`](./meta.json).

## Esquema de cada elemento

```json
{
  "id": "px-<id estable>",
  "company": "Acme Corp",
  "title": "Financial Analyst",
  "url": "https://...",
  "apply_url": "https://...",
  "posted": "2026-09-10T14:03:00Z",
  "location": "Los Angeles, CA",
  "salary": "$70K-$85K",
  "description": "primeros 1500 caracteres, texto plano",
  "source": "perplexity/<greenhouse|ashby|lever|workday|eightfold>",
  "applicants": null
}
```

## Diferencias con el contrato original acordado

Se acordó un endpoint `GET /jobs?since_hours=48&limit=500` con header `Authorization: Bearer <token>`. Por limitaciones de infraestructura (ver hilo de Perplexity), en esta primera versión se sirve como archivo estático en GitHub en vez de servidor HTTP dinámico:

- No soporta `since_hours`/`limit` dinámicos: siempre representa el snapshot fijo de 48h / máx. 500, que es exactamente el valor que usa el pull diario.
- No requiere/valida `Authorization: Bearer`: el repositorio es público porque el contenido son vacantes públicas (no hay datos sensibles).
- El repositorio es público y de solo lectura; el token generado para el contrato original queda reservado para cuando se migre a un servidor real (Vercel/Render).

## Fuentes originales cubiertas (para evitar doble conteo)

- **Greenhouse**: pinterest, waymo, airbnb, coinbase, robinhood, stripe, reddit, lyft, cloudflare, databricks, scaleai
- **Ashby**: openai, perplexity, ramp, linear, notion
- **Lever**: palantir
- **Workday**: disney (incluye ESPN en el mismo feed)
- **Eightfold**: netflix
