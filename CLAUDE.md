# 4minkm

Plan de entrenamiento para bajar de 20' en 5 km (Jekyll, en español). Los entrenos van en `entrenos-realizados-fits/AAAAMMDD/` (una subcarpeta por día).

**Antes de analizar cualquier .fit, lee [notas-sobre-los-datos.md](notas-sobre-los-datos.md).** Resumen:

- Nunca uses `total_ascent`: el barómetro lo infla. Para el desnivel, contrasta el GPS con el DEM (API de elevación de Open-Meteo, con curl).
- Horas del .fit en UTC → +2 h (+1 h desde el 25/10).
- La cadencia del .fit es por pierna → ×2.
- No saques conclusiones de forma comparando sesiones distintas.
- Si detectas un error nuevo en los datos o en el análisis, añádelo a `notas-sobre-los-datos.md`.

## Trabajar con Claude Code

- **Límite de ~50.000 caracteres por mensaje**: si se pasa, el mensaje llega cortado y se pierde lo dictado. Antes de mandar un mensaje largo (dictado por voz), cópialo a un bloc de notas. Si llega cortado, dilo y pide el resto.
- Este repositorio es **público**, pero Raúl ha decidido que las conversaciones con el entrenador de KM (`km/conversaciones/`) **sí se suben**: tras cada sincronización, commit y push de todo `km/` (conversaciones incluidas). Que no se olvide. La app KM (privada) es [moix-top/km](https://github.com/moix-top/km).
- `km/` se genera con `scripts/sync-4minkm.ts` del repositorio de KM: no se edita a mano.
