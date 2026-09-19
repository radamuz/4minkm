# 4minkm

Plan de entrenamiento para bajar de 20' en 5 km (Jekyll, en español). Los entrenos van en `entrenos-realizados-fits/`.

**Antes de analizar cualquier .fit, lee [notas-sobre-los-datos.md](notas-sobre-los-datos.md).** Resumen:

- Nunca uses `total_ascent`: el barómetro lo infla. Para el desnivel, contrasta el GPS con el DEM (API de elevación de Open-Meteo, con curl).
- Horas del .fit en UTC → +2 h (+1 h desde el 25/10).
- La cadencia del .fit es por pierna → ×2.
- No saques conclusiones de forma comparando sesiones distintas.
- Si detectas un error nuevo en los datos o en el análisis, añádelo a `notas-sobre-los-datos.md`.
