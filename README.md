# when-to-look-up

> Know exactly when to step outside to catch a colorful sky.

A single-file web app that scores upcoming sunrises and sunsets on a 0–100 scale based on the atmospheric conditions that actually produce vivid sky color — cloud layering, humidity, and visibility. No account, no API key, no build step.

**[Live demo →](https://your-username.github.io/when-to-look-up/)** *(update after deploy)*

---

## How it works

Vivid sunrises and sunsets need two things at once: a **lit canvas** of mid and high clouds for low-angle sunlight to bounce off, and a **clear horizon** so that light can actually reach them. The app pulls hourly forecast data from [Open-Meteo](https://open-meteo.com) and combines five signals into a score for each event:

| Signal | Weight | Ideal range | Why it matters |
|---|---|---|---|
| Mid-level cloud cover | 30% | 30–60% | Altocumulus/altostratus at 2–6 km catch the underside of low-angle light — the primary driver of dramatic color |
| High cloud cover | 25% | 30–70% | Cirrus catches the very first and very last light, producing pink and violet tones |
| Low cloud cover | 20% | < 20% (penalty) | Stratus and fog sit at the horizon and block the light path before it reaches the upper canvas |
| Surface humidity | 15% | < 40% | High humidity scatters light diffusely and washes out saturation |
| Visibility | 10% | > 15 km | A proxy for aerosol and particulate load — cleaner air, more vivid colors |

Sunrise and sunset times come from Open-Meteo's daily astronomy fields, computed from the NOAA solar position algorithm — accurate to the minute for any coordinates on Earth. The highlighted viewing window is **±25 minutes** around the peak time, covering golden hour through early blue hour.

## Scoring tiers

| Score | Label |
|---|---|
| 80–100 | Spectacular |
| 65–79 | Vibrant |
| 50–64 | Pleasant |
| 35–49 | Mediocre |
| 0–34 | Drab |

## Running locally

No dependencies, no build step — just open the file:

```bash
open index.html
```

Or serve it with anything:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Data sources

- [`api.open-meteo.com/v1/forecast`](https://open-meteo.com/en/docs) — hourly weather + daily sunrise/sunset
- [`geocoding-api.open-meteo.com/v1/search`](https://open-meteo.com/en/docs/geocoding-api) — city name → coordinates

Both endpoints are free for non-commercial use, require no API key, and have CORS enabled. Attribution is included in the page footer per Open-Meteo's terms.

## Privacy

No tracking, no cookies, no analytics, no ads. If you grant location access, your coordinates are sent only to Open-Meteo's API to fetch the local forecast — they are never stored or logged by this app.

## License

MIT