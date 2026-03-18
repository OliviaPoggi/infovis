# Guía de Inicio Rápido

## 🚀 Primeros pasos con el dataset

### 1. Exploración Básica

#### Python
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Cargar datos
viewing = pd.read_csv('data/viewing_activity.csv')
search = pd.read_csv('data/search_history.csv')

# Convertir fechas
viewing['Start Time'] = pd.to_datetime(viewing['Start Time'])
search['Utc Timestamp'] = pd.to_datetime(search['Utc Timestamp'])

# Ver primeras filas
print(viewing.head())
print(viewing.info())
```

### 2. Análisis por Perfil

```python
# Horas totales por perfil
hours_by_profile = viewing.groupby('Profile Name')['Duration_Hours'].sum().sort_values(ascending=False)
print(hours_by_profile)

# Visualizaciones por perfil
views_by_profile = viewing['Profile Name'].value_counts()
print(views_by_profile)
```

### 3. Análisis Temporal

```python
# Visualizaciones por año
viewing['Year'] = viewing['Start Time'].dt.year
yearly_views = viewing.groupby('Year').size()

# Gráfico
yearly_views.plot(kind='bar', figsize=(10, 6), title='Visualizaciones por Año')
plt.ylabel('Visualizaciones')
plt.show()
```

### 4. Análisis de Contenido

```python
# Top 10 series más vistas
top_series = viewing[viewing['Content_Type'] == 'Serie']['Series_Name'].value_counts().head(10)
print(top_series)

# Distribución de tipos de contenido
content_dist = viewing['Content_Type'].value_counts()
content_dist.plot(kind='pie', autopct='%1.1f%%', figsize=(8, 8))
plt.title('Distribución de Tipos de Contenido')
plt.show()
```

### 5. Análisis de Horarios

```python
# Horarios de visualización por perfil
import seaborn as sns

plt.figure(figsize=(12, 6))
for profile in ['olu', 'UMA', 'SIL']:
    profile_data = viewing[viewing['Profile Name'] == profile]
    hourly_views = profile_data['Hour'].value_counts().sort_index()
    plt.plot(hourly_views.index, hourly_views.values, marker='o', label=profile)

plt.xlabel('Hora del día')
plt.ylabel('Visualizaciones')
plt.title('Patrones de Horario por Perfil')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### 6. Análisis de Búsquedas

```python
# Top búsquedas por perfil
for profile in ['olu', 'UMA', 'SIL']:
    print(f"\n=== {profile} ===")
    top_searches = search[search['Profile Name'] == profile]['Query Typed'].value_counts().head(10)
    print(top_searches)
```

## 📊 Visualizaciones Recomendadas

### Heatmap de Actividad Semanal

```python
# Crear pivot table para heatmap
viewing['DayOfWeek'] = viewing['Start Time'].dt.day_name()
heatmap_data = viewing.pivot_table(
    values='Duration_Minutes', 
    index='DayOfWeek', 
    columns='Hour', 
    aggfunc='sum',
    fill_value=0
)

# Ordenar días de la semana
day_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
heatmap_data = heatmap_data.reindex(day_order)

# Crear heatmap
plt.figure(figsize=(14, 6))
sns.heatmap(heatmap_data, cmap='YlOrRd', cbar_kws={'label': 'Minutos'})
plt.title('Mapa de Calor de Actividad por Día y Hora')
plt.xlabel('Hora del día')
plt.ylabel('Día de la semana')
plt.show()
```

### Evolución Temporal por Perfil

```python
# Visualizaciones mensuales por perfil
monthly_data = viewing.groupby(['YearMonth', 'Profile Name']).size().unstack(fill_value=0)

monthly_data.plot(figsize=(14, 6), marker='o')
plt.title('Evolución Mensual de Visualizaciones por Perfil')
plt.xlabel('Mes')
plt.ylabel('Visualizaciones')
plt.legend(title='Perfil')
plt.grid(True, alpha=0.3)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

## 🎯 Ideas de Análisis Avanzado

1. **Análisis de Sesiones**
   - Calcular sesiones de binge-watching
   - Identificar patrones de maratón

2. **Análisis de Dispositivos**
   - Preferencias de dispositivo por tipo de contenido
   - Movilidad entre dispositivos

3. **Análisis Predictivo**
   - Predecir qué contenido verá cada perfil
   - Predecir horarios de máxima actividad

4. **Network Analysis**
   - Grafo de contenido relacionado
   - Recomendaciones basadas en patrones

5. **Clustering**
   - Agrupar días similares de consumo
   - Identificar patrones de comportamiento

## 💡 Tips

- Usa `Content_Type` para filtrar series vs películas
- Las columnas `Duration_Minutes` y `Duration_Hours` ya están en formato numérico
- `Time_Category` categoriza automáticamente en Mañana/Tarde/Noche/Madrugada
- `Series_Name` extrae el nombre de la serie de títulos de episodios
- Algunos títulos pueden estar como "Unknown Title" - filtra si es necesario

## 🔗 Recursos Adicionales

- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Matplotlib Gallery](https://matplotlib.org/stable/gallery/index.html)
- [Seaborn Tutorial](https://seaborn.pydata.org/tutorial.html)
