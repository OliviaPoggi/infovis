# Netflix Viewing Analysis (2020-2026)

Análisis de patrones de consumo de Netflix para tres usuarios durante el período 2020-2026.

## 📊 Descripción del Dataset

Este dataset contiene el historial completo de visualizaciones y búsquedas de Netflix de tres perfiles de usuario durante un período de 6 años (2020-2026).

### Archivos

- **`data/viewing_activity.csv`** - Historial de visualizaciones (19,757 registros)
- **`data/search_history.csv`** - Historial de búsquedas (1,265 registros)
- **`data/dataset_stats.json`** - Estadísticas resumidas del dataset

## 👥 Perfiles de Usuario

- **UMA**: 14,382 visualizaciones (72.8% del total) - La maratonera
- **olu**: 4,317 visualizaciones (21.9% del total) - La fan de comedia
- **SIL**: 1,058 visualizaciones (5.4% del total) - La cinéfila selectiva

## 📈 Estadísticas Clave

- **Total de visualizaciones**: 19,757
- **Total de búsquedas**: 1,265
- **Período**: 2020-01-01 a 2026-03-07
- **Horas totales**: ~5,513 horas
- **Contenido**: 91.5% Series, 6.0% Películas, 2.5% Trailers/Clips

### Distribución por Año

- 2020: 5,241 visualizaciones (26.5%) - Pico pandémico
- 2021: 3,524 visualizaciones
- 2022: 2,370 visualizaciones
- 2023: 2,192 visualizaciones
- 2024: 3,126 visualizaciones
- 2025: 3,058 visualizaciones
- 2026: 246 visualizaciones (parcial)

## 📁 Estructura de Datos

### viewing_activity.csv

Columnas principales:

**Información básica:**
- `Profile Name` - Nombre del perfil (olu/UMA/SIL)
- `Title` - Título del contenido
- `Start Time` - Fecha y hora de inicio
- `Duration` - Duración (formato HH:MM:SS)
- `Device Type` - Dispositivo usado
- `Country` - País de visualización

**Columnas enriquecidas:**
- `Duration_Minutes` - Duración en minutos (numérico)
- `Duration_Hours` - Duración en horas (numérico)
- `Content_Type` - Tipo de contenido (Serie/Película/Trailer-Clip)
- `Series_Name` - Nombre de la serie (extraído automáticamente)
- `YearMonth` - Período año-mes (ej: 2020-01)
- `DayOfWeek` - Día de la semana (Monday, Tuesday...)
- `DayOfWeek_Num` - Día numérico (0=Lunes, 6=Domingo)
- `Hour` - Hora del día (0-23)
- `Month` - Mes (1-12)
- `Quarter` - Trimestre (1-4)
- `Date` - Fecha
- `Time_Category` - Categoría temporal (Mañana/Tarde/Noche/Madrugada)
- `Country_Clean` - País limpio (Argentina, Spain...)

### search_history.csv

Columnas principales:
- `Query Typed` - Búsqueda realizada
- `Profile Name` - Nombre del perfil
- `Displayed Name` - Título mostrado
- `Utc Timestamp` - Fecha y hora
- `Action` - Acción (play/select)
- `Device` - Dispositivo usado
- `YearMonth` - Período año-mes

## 🎯 Posibles Análisis

Este dataset permite realizar diversos análisis:

1. **Análisis de comportamiento de usuario**
   - Patrones de horarios de visualización
   - Preferencias de contenido por perfil
   - Dispositivos más utilizados

2. **Análisis temporal**
   - Evolución del consumo año tras año
   - Estacionalidad y tendencias
   - Impacto de eventos (ej: pandemia en 2020)

3. **Análisis de contenido**
   - Series más populares por perfil
   - Ratio series vs películas
   - Géneros preferidos

4. **Análisis geográfico**
   - Países de visualización
   - Patrones de movilidad

5. **Análisis de búsquedas**
   - Términos más buscados
   - Relación entre búsquedas y visualizaciones

## 🔍 Insights Principales

- **UMA domina el consumo** con el 72.8% de todas las visualizaciones
- **Todos los perfiles son nocturnos** (picos entre 23:00-03:00)
- **2020 fue el año de mayor consumo** (26.5% del total) por el confinamiento
- **Las series dominan** sobre las películas (91.5% vs 6.0%)
- **Cada perfil tiene preferencias únicas**:
  - UMA: Rebelde Way (telenovela adolescente)
  - olu: The Office (comedia situacional)
  - SIL: The Crown (drama de época)

## 🛠️ Uso del Dataset

### Python (pandas)

```python
import pandas as pd

# Cargar datos
viewing = pd.read_csv('data/viewing_activity.csv')
search = pd.read_csv('data/search_history.csv')

# Convertir fechas
viewing['Start Time'] = pd.to_datetime(viewing['Start Time'])

# Análisis básico
print(viewing.groupby('Profile Name')['Duration_Hours'].sum())
```

### R

```r
library(readr)
library(dplyr)

# Cargar datos
viewing <- read_csv('data/viewing_activity.csv')

# Análisis básico
viewing %>% 
  group_by(Profile_Name) %>% 
  summarise(total_hours = sum(Duration_Hours))
```

## 📝 Notas

- Los datos están anonimizados (solo nombres de perfil, sin información personal)
- El período 2026 está incompleto (solo hasta marzo)
- Algunos títulos aparecen como "Unknown Title" en los datos originales
- Las duraciones son aproximadas basadas en el formato HH:MM:SS

## 📄 Licencia

Este dataset se proporciona con fines educativos y de análisis.

## 🤝 Contribuciones

Si encuentras errores o tienes sugerencias para mejorar el dataset, abre un issue.

---

**Última actualización**: 2026-03-18
