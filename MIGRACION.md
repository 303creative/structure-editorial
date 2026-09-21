# Migración Structure — poner A en vivo y deshabilitar B

**A (nueva):** repo `structure-editorial` → hoy en `structure-editorial.vercel.app`
**B (actual):** repo `structure-mens-studio-web` → hoy sirve `structuremenstudio.com`
**Dominio final:** `https://structuremenstudio.com` (SIN www)

El código ya está listo para el cambio. La conmutación real del dominio se hace en Vercel (pasos abajo).

## 1. Push de los dos repos (GitHub Desktop)
- Repo **A** (`structure-editorial`): confirmar + push. (Trae dominio non‑www, vercel.json con www→non‑www e /index.html→/, tienda noindex, anclas, fallback sin‑JS, precios en home, reseñas reales, etc.)
- Repo **B** (`structure-mens-studio-web`): confirmar + push. (Solo añade `vercel.json`: redirige a la web viva en cualquier host que no sea el dominio final; nunca hace bucle.)

## 2. Cambiar el dominio en Vercel (esto es lo que pone A en vivo)
1. Proyecto **B** → Settings → Domains → quitar `structuremenstudio.com` **y** `www.structuremenstudio.com`.
2. Proyecto **A** → Settings → Domains → añadir `structuremenstudio.com` (marcarlo como principal) y añadir `www.structuremenstudio.com` como redirect a la raíz.
3. Esperar a que Vercel propague el certificado/redirección.

> Al quitar el dominio de B, el `vercel.json` de B empieza a redirigir su URL `*.vercel.app` a la web viva → B queda deshabilitada para el público. A pasa a servir el dominio y deja de estar en `noindex` (el noindex solo se aplica en `*.vercel.app`).

## 3. Comprobaciones en producción (mismo día)
- `https://structuremenstudio.com/` carga la Web A (200) con canonical `https://structuremenstudio.com/`.
- `https://www.structuremenstudio.com/` → redirige 308 a sin www (sin bucle).
- `https://structuremenstudio.com/index.html` → redirige a `/`.
- Cargan 200: `/services.html`, `/scissor-haircut-brickell.html`, `/work.html`, `/barbers.html`, `/journal.html`, `/reviews.html`, `/policies.html` y los 8 artículos + `/asian-mens-haircut-miami-brickell.html`.
- Exportaciones viejas redirigen 308: `/Blog.dc.html`→`/journal.html`, `/Merch.dc.html`→`/store.html`, las dos `Structure...v1/.dc.html`→`/`.
- `robots.txt` y `sitemap.xml` responden; `store.html` y `chat.html` en `noindex`; el resto indexable.
- La URL vieja de B (`structure-mens-studio-web.vercel.app`) redirige a la web viva.

## 4. Post‑lanzamiento (tú, con datos)
- Google Search Console: reenviar `sitemap.xml`, inspeccionar unas URLs, confirmar propiedad.
- Probar la reserva de Squire en un móvil real (iPhone Safari + Android Chrome).
- Confirmar en la cuenta de Squire precios/servicios/duraciones (y si hay servicios de barba/afeitado/line‑up vigentes, avisar para añadirlos).
- GBP: horario real, indicaciones/Suite 4, foto de interior, correo `info@structuremenstudio.com`.

## Reversión rápida
Si algo crítico falla (home inaccesible, bucle de dominio, reserva rota en dos navegadores): en Vercel, volver a poner `structuremenstudio.com` en el proyecto **B**. El `vercel.json` de B no redirige en ese dominio, así que la web antigua vuelve a servirse tal cual.
