# Notas sobre los datos

Qué datos de los .fit son fiables, cuáles no, y errores ya cometidos al analizarlos. **Leer antes de sacar conclusiones de cualquier entreno nuevo.**

Reloj: Garmin (producto 3282), con altímetro barométrico. Las horas del .fit van en **UTC**: en septiembre–octubre hay que sumar **+2 h** (+1 h desde el 25/10).

## Resumen: qué creerse

| Dato | Fiable | Notas |
|---|---|---|
| Distancia y ritmo corriendo al aire libre | ✅ | GPS, error típico ±1–2 % |
| Tiempo (timer) | ✅ | Ojo: `total_timer_time` ≠ `total_elapsed_time` si hubo pausas |
| Pulso (medio y por zonas) | ✅ | Sin saltos raros en ningún .fit |
| Cadencia | ✅ | El .fit la guarda **por pierna**: multiplicar por 2 (79 → 158 spm) |
| **Desnivel (`total_ascent`)** | ❌ | Ver error 1 |
| Distancia en el gimnasio (híbrido) | ❌ | GPS bajo techo: no mide ski, remo, bici ni trineo |
| Velocidad máxima | ⚠️ | Picos de GPS: 23,9 km/h en los strides no es creíble |

## Errores cometidos

### 1. Desnivel del reloj tomado como real (detectado el 19/09)

Se usó `total_ascent` del .fit sin contrastarlo. Dijimos que el 5 km del 09/09 tenía **137 m de subida** ("bastante") y que en llano habría salido en 21:30–21:45. Era falso.

- El barómetro suma oscilaciones de 1–3 m (viento, presión, bolsillo) y en rutas llanas da cifras absurdas: 129–169 m en el paseo marítimo, con altitudes negativas.
- Primero se intentó filtrar el ruido de la altitud del reloj (umbral + suavizado), pero **el resultado dependía por completo del filtro**: entre 20 y 120 m según los parámetros. Con eso se estimaron "~50 m" para el 09/09, que también estaba mal.
- **Contraste con el modelo del terreno** (Open-Meteo elevation API, Copernicus DEM de 90 m; un punto cada 50 m del GPS):

| Entreno | Reloj | DEM: subida | DEM: altitud mín–máx | Conclusión |
|---|---|---|---|---|
| 5 km al máximo (09/09) | 137 m | ~13 m | 38–56 m (**−17 m neto**) | Casi llano y **cuesta abajo** |
| 5 km tranquilo (15/09) | 77 m | — | 0–7 m | Llano |
| 30' Z2 (17/09) | 169 m | — | 0–14 m | Llano |
| 8 km con Javi (19/09) | 129 m | — | 0–7 m | Llano |

  En las rutas del paseo el DEM también da "subidas" de 60–95 m, pero son saltos de 0 a 5 m entre celdas de mar y tierra de 90 m: ruido. Todo el recorrido está entre 0 y 7–14 m, así que es llano.

- **Consecuencias que se corrigieron en el plan**:
  - La marca en llano no es 21:30–21:45 sino **~22:35**, porque la bajada ayudó.
  - El 4:15 del último km no fue una bajada (−3 m): fue sprint. Los km 1–2 sí fueron cuesta abajo (−17 m).
  - El km 4 (4:48) se hundió en llano, por cansancio.

**Regla**: nunca usar `total_ascent`. Para el desnivel, contrastar con el DEM:

```python
# lat, lon del .fit: semicírculos × 180 / 2**31
# hasta 100 puntos por petición, 1 punto cada ~50 m
https://api.open-meteo.com/v1/elevation?latitude=39.55,39.56&longitude=2.69,2.70
```

(`api.opentopodata.org` está bloqueado desde este entorno. `urllib` de Python falla por certificado, pero `curl` funciona.)

### 2. Conclusiones de forma con una sola sesión

Comparar el híbrido o las pachangas de un viernes con el anterior **no mide la forma**: son sesiones distintas (formato, duración, pareja, descansos). Por ejemplo, la FC media de las pachangas del 18/09 (170) salió más alta que la del 11/09 (157) solo porque el 11/09 hubo 34' casi parado al final.

**Regla**: la forma solo se mide con los controles de carrera (simulación 27/09 y test 18/10) o comparando rodajes en la misma ruta a la misma FC.

### 3. Detalles menores corregidos

- La cadencia se escribía a veces en "ppm": es **spm** (pasos por minuto).
- El rodaje del 17/09 se describió como "a 6:15". La media fue de 6:42 (km 1–3 a ~6:20, luego más lento). Además, el reloj estuvo **11 minutos en pausa** (timer 38' frente a 49' en total).

## Cómo analizar un .fit nuevo (checklist)

1. Pasar las horas a hora local (+2 h / +1 h).
2. Parciales por km: tiempo, FC media/máx y cadencia × 2.
3. Tiempo en zonas: < 150 · 150–172 · 172–185 · > 185.
4. **Desnivel con DEM**, nunca el del reloj. Mirar también la altitud neta por km: cuesta abajo también cuenta.
5. Comprobar si hubo pausas (timer frente a elapsed).
6. En el gimnasio: ignorar distancia, ritmo y desnivel. Solo cuentan el pulso y el tiempo.
