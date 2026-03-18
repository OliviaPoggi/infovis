# Data Dictionary

## viewing_activity.csv

Descripción completa de todas las columnas en el archivo de visualizaciones.

### Columnas Originales

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `Duration` | String | Duración en formato HH:MM:SS | "00:45:30" |
| `Start Time` | DateTime | Fecha y hora de inicio de visualización | "2020-03-15 22:30:45" |
| `Profile Name` | String | Nombre del perfil (olu/UMA/SIL) | "UMA" |
| `Country` | String | País de visualización con código | "AR (Argentina)" |
| `Bookmark` | String | Marca de tiempo donde se detuvo | "00:23:15" |
| `Latest Bookmark` | String | Última posición o "Not latest view" | "00:45:30" |
| `Supplemental Video Type` | String | Tipo de contenido suplementario | "TRAILER", "" |
| `Attributes` | String | Atributos adicionales | "Autoplayed: user action: None;" |
| `Device Type` | String | Dispositivo utilizado | "Apple iPhone 14 iPhone" |
| `Title` | String | Título del contenido | "Bridgerton: Temporada 2: Episodio 1" |

### Columnas Enriquecidas

| Columna | Tipo | Descripción | Valores Posibles | Ejemplo |
|---------|------|-------------|------------------|---------|
| `Duration_Minutes` | Float | Duración en minutos | 0 - 300+ | 45.5 |
| `Duration_Hours` | Float | Duración en horas | 0 - 5+ | 0.758 |
| `Content_Type` | String | Tipo de contenido | Serie, Película, Trailer/Clip | "Serie" |
| `Series_Name` | String | Nombre de la serie (solo series) | Texto o NULL | "Bridgerton" |
| `YearMonth` | String | Período año-mes | YYYY-MM | "2020-03" |
| `DayOfWeek` | String | Día de la semana | Monday - Sunday | "Friday" |
| `DayOfWeek_Num` | Integer | Día numérico | 0-6 (0=Monday) | 4 |
| `Hour` | Integer | Hora del día | 0-23 | 22 |
| `Month` | Integer | Mes | 1-12 | 3 |
| `Quarter` | Integer | Trimestre | 1-4 | 1 |
| `Date` | Date | Fecha sin hora | YYYY-MM-DD | "2020-03-15" |
| `Time_Category` | String | Categoría temporal | Mañana, Tarde, Noche, Madrugada | "Noche" |
| `Country_Clean` | String | País limpio | Nombre del país | "Argentina" |

### Categorización de Time_Category

- **Mañana**: 06:00 - 11:59
- **Tarde**: 12:00 - 17:59
- **Noche**: 18:00 - 23:59
- **Madrugada**: 00:00 - 05:59

### Lógica de Content_Type

El tipo de contenido se determina así:

1. **Serie**: El título contiene "Temporada", "temporada", "episodio", "Episode", "Season" o "Miniserie"
2. **Trailer/Clip**: El título contiene "Trailer", "Tráiler", "Teaser", "Avance", "Clip" o "hook"
3. **Película**: Todo lo demás

### Extracción de Series_Name

Para contenido tipo "Serie", se extrae el nombre así:
- Si hay `:` en el título, toma todo antes del primer `:`
- Ejemplo: "Bridgerton: Temporada 2: Episodio 1" → "Bridgerton"

---

## search_history.csv

Descripción completa de todas las columnas en el archivo de búsquedas.

### Columnas Originales

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `Query Typed` | String | Búsqueda ingresada por el usuario | "bridgerton" |
| `Profile Name` | String | Nombre del perfil | "UMA" |
| `Country Iso Code` | String | Código ISO del país | "AR" |
| `Displayed Name` | String | Título mostrado en resultados | "Bridgerton" |
| `Utc Timestamp` | DateTime | Fecha y hora de la búsqueda | "2020-03-15 22:30:45" |
| `Action` | String | Acción realizada | "play", "select" |
| `Is Kids` | String | Si es perfil de niños | "0", "1" |
| `Section` | String | Sección de búsqueda | "title_results" |
| `Device` | String | Dispositivo usado | "iPhone" |

### Columnas Enriquecidas

| Columna | Tipo | Descripción | Valores Posibles | Ejemplo |
|---------|------|-------------|------------------|---------|
| `YearMonth` | String | Período año-mes | YYYY-MM | "2020-03" |
| `Month` | Integer | Mes | 1-12 | 3 |

---

## dataset_stats.json

Archivo JSON con estadísticas resumidas del dataset.

### Estructura

```json
{
  "dataset_info": {
    "title": "Título del dataset",
    "description": "Descripción",
    "period": "Rango de fechas",
    "profiles": ["lista", "de", "perfiles"],
    "total_views": 19757,
    "total_searches": 1265,
    "last_updated": "2026-03-18"
  },
  "viewing_activity": {
    "total_records": 19757,
    "columns": ["lista", "de", "columnas"],
    "date_range": {
      "start": "fecha inicio",
      "end": "fecha fin"
    },
    "by_profile": {
      "UMA": {
        "views": 14382,
        "total_hours": 3862.9,
        "avg_session_minutes": 16.1
      }
    },
    "by_year": {
      "2020": 5241,
      "2021": 3524
    },
    "by_content_type": {
      "Serie": 18078,
      "Película": 1186
    }
  },
  "search_history": {
    "total_records": 1265,
    "columns": ["lista", "de", "columnas"],
    "date_range": {
      "start": "fecha inicio",
      "end": "fecha fin"
    }
  }
}
```

---

## Notas Importantes

### Datos Faltantes

- Algunos títulos aparecen como "Unknown Title" en los datos originales
- `Series_Name` es NULL para películas y trailers
- `Country_Clean` puede ser NULL si no se pudo extraer

### Consideraciones de Privacidad

- Los datos están anonimizados
- Solo contienen nombres de perfil (olu, UMA, SIL)
- No hay información personal identificable
- Las direcciones IP no están incluidas

### Calidad de Datos

- Las duraciones están en formato HH:MM:SS y se convirtieron a numérico
- Algunas visualizaciones tienen duración 0 (probablemente previews)
- Los dispositivos están en texto libre (puede haber variaciones)
- Los países están normalizados en `Country_Clean`

### Uso Recomendado

Para análisis, se recomienda usar las columnas enriquecidas:
- `Duration_Minutes` o `Duration_Hours` en lugar de `Duration`
- `Content_Type` para filtrar
- `Series_Name` para agrupar series
- `YearMonth` para análisis temporal
- `Time_Category` para análisis de horarios
