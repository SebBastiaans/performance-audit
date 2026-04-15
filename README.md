# Performance Audit 

Doe [een Performance audit](https://github.com/SebBastiaans/performance-audit/wiki/Performance%E2%80%90audit#webpagetest) op een bestaande website uit je eigen omgeving en rapporteer daarover.

De instructies van deze opdracht staan in [INSTRUCTIONS](https://github.com/fdnd-task/performance-audit/blob/main/docs/INSTRUCTIONS.md).

# Performance Audit — maris-care.nl

Een performance audit uitgevoerd op [maris-care.nl](https://maris-care.nl/), een bedrijf waarvoor ik regelmatig werk uitvoer.

---

## Gebruikte tools

| Tool | Doel |
|---|---|
| Lighthouse (Chrome DevTools) | Geautomatiseerde performance score + diagnostics |
| PageSpeed Insights | Vergelijking met Real User Metrics (CrUX) |
| WebPageTest | Diepgaande waterfall-analyse |

---

## Bevindingen per tool

### 1. [Lighthouse](https://github.com/SebBastiaans/performance-audit/wiki/Performance%E2%80%90audit#lighthouse-performance-test)

**[Mobile score](https://github.com/SebBastiaans/performance-audit/wiki/Performance%E2%80%90audit#mobile): laag — [Desktop score](https://github.com/SebBastiaans/performance-audit/wiki/Performance%E2%80%90audit#desktop): hoger**

De grootste problemen op beide apparaten:

- 🔴 **Reduce unused JavaScript** — ~376 KiB aan ongebruikte JS wordt ingeladen. Het grootste knelpunt van de site. Oorzaak: grote libraries waarvan maar een klein deel gebruikt wordt.
- 🔴 **Reduce unused CSS** — ~59 KiB aan CSS die de browser verwerkt maar nooit gebruikt.
- 🟠 **Avoid enormous network payloads** — De totale paginagrootte is ruim 4 MB op mobile en nog zwaarder op desktop.
- ⚪ **Avoid long main-thread tasks** — Op mobile zijn er 10 taken die langer dan 50ms duren, waardoor de pagina tijdelijk "bevriest".

### 2. [PageSpeed Insights](https://github.com/SebBastiaans/performance-audit/wiki/Performance%E2%80%90audit#pagespeed-insights)

De scores komen grotendeels overeen met Lighthouse, maar de **network payload is op PageSpeed Insights bijna het dubbele** vergeleken met de lokale Lighthouse test. Dit komt doordat PageSpeed Insights de test uitvoert vanaf externe servers en meer realistische netwerkomstandigheden simuleert.

Op desktop zijn de long main-thread tasks hier nog wél zichtbaar (7 taken), terwijl die in de lokale Lighthouse test verdwenen waren.

### 3. [WebPageTest](https://github.com/SebBastiaans/performance-audit/wiki/Performance%E2%80%90audit#webpagetest)

Opvallende patronen:
- De waterfall-analyse toont **96 requests**. 
- **Grote afbeeldingen laat in de waterfall** (rijen 43–59) — afbeeldingen worden pas laat geladen, wat de Largest Contentful Paint (LCP) negatief beïnvloedt.
- **Google Fonts & Gstatic** (rijen 19, 63–66) — externe font-requests voegen extra roundtrips toe.

---

## Conclusie

Het grootste performance probleem van maris-care.nl is de hoeveelheid **ongebruikte JavaScript** (~376 KiB).

**Verbeteringen met de meeste impact:**

1. **Code splitting / lazy loading**
2. **Afbeeldingen optimaliseren**
3. **Self-host fonts**
4. **CSS opschonen** 
5. **Gzip/Brotli compressie**

## Licentie

This project is licensed under the terms of the [MIT license](./LICENSE).
