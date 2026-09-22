# cellerirealestate-assets

Repositorio público de media de cellerirealestate.com, servido a través de jsDelivr para que el sitio no consuma ancho de banda ni cuota de optimización de imágenes de Vercel.

## Para quién es

Repositorio público de media servido por jsDelivr, consumido por `apps/web` y `apps/cotizaolonesa`. No contiene código de aplicación.

## Patrón de URL

```
https://cdn.jsdelivr.net/gh/realwallker/cellerirealestate-assets@<tag>/<path>
```

Publicar un tag nuevo por cada cambio para que los archivos cacheados por jsDelivr permanezcan inmutables.

## Estructura del proyecto

```
fonts/            tipografías en .ttf usadas por el documento de cotización (Hanken Grotesk, Libre Caslon Display)
fotos/reales/      renders aprobados de Olonesa Reserva Village, en WebP
fotos/stock/       fotos de apoyo (Pexels / Unsplash), en WebP
marca/icons/       favicon y apple-touch-icon
marca/web/         logos recortados para el sitio (Celleri horizontal/símbolo/vertical/wordmark, Olonesa Reserva Village)
masterplan/        raster del plan maestro y sus variantes WebP
masterplan/lotes/   bandas del plano recortadas por lote, usadas en el documento de cotización
```

Las fotos existen como WebP en anchos 256, 640, 1080, 1600 y 2400 px (`<nombre>-<ancho>.webp`, nunca escaladas hacia arriba desde el original). Generadas por `apps/web/scripts/build-cdn-assets.mjs` a partir de los archivos fuente en `assets/`.

## Fuente de verdad de los datos

Este repositorio no contiene datos de negocio (precios, disponibilidad, plan de pagos): esos viven en `apps/web/src/content/olonesa-lots.ts` y equivalentes, con "ORV Financiamiento Oficial.pdf" como única fuente de verdad. Este repo solo sirve la media asociada al masterplan y a la marca.

## Desarrollo local

No aplica: este repositorio no tiene build ni servidor propio. Los archivos se generan en `apps/web` (`pnpm cdn:build`) y se commitean aquí.

## Deploy

No hay despliegue: jsDelivr sirve directamente el contenido de GitHub por tag. El flujo es:

1. Generar o actualizar los archivos fuente en `assets/` (workspace `Celleri-CC`).
2. Desde `apps/web`: `pnpm cdn:build` — escribe las variantes WebP aquí.
3. Commitear los cambios en este repo y subir un tag nuevo.
4. Actualizar ese tag en `apps/web/src/lib/cdn.ts` y `apps/cotizaolonesa/src/lib/cdn.ts`.

Nota: `apps/cotizador` usa un repositorio de media distinto (`realwallker/cotizador-assets`), no este.

## Relación con los otros repos

Parte del workspace `Celleri-CC` (`apps/` + `assets/` + `cdn-assets/`):

- `assets/`: fuente de verdad de los archivos originales (sin repo propio); `apps/web/scripts/build-cdn-assets.mjs` los convierte y los escribe aquí.
- `apps/web`: consume este repo vía jsDelivr para toda la media de la landing (`src/lib/cdn.ts`, `src/lib/image-loader.ts`); ningún asset se despliega junto con la app.
- `apps/cotizaolonesa`: consume el mismo repo y el mismo patrón de tag (`src/lib/cdn.ts`), para los logos, el masterplan y las bandas del plano.
- `apps/cotizador`: no consume este repo; tiene su propio repositorio de media (`cotizador-assets`).

## Mantenimiento

- Agregar media nueva: colocar el archivo fuente en `assets/` (subcarpeta correspondiente), correr `pnpm cdn:build` desde `apps/web`, commitear aquí y subir un tag nuevo.
- Actualizar el tag nuevo en `apps/web/src/lib/cdn.ts` y `apps/cotizaolonesa/src/lib/cdn.ts` para que las apps sirvan los archivos publicados.
- Renders © PCC Promotora / Olonesa Reserva Village. Fotos de stock bajo licencias de Pexels / Unsplash.
