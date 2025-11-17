# Proyecto: Modelo Analítico Climático para Riesgo de Café en Colombia  
**README.md**

---

## 1. Contexto del Proyecto

El cultivo de café en Colombia es altamente vulnerable al riesgo climático. La variabilidad en precipitaciones, la ocurrencia de lluvias extremas y el impacto sobre las etapas críticas del ciclo productivo generan pérdidas frecuentes para los caficultores.

En el marco del curso *Gerencia de Proyectos Analíticos*, este proyecto desarrolla un enfoque analítico para estudiar la relación entre la precipitación y el riesgo productivo del café, como base para un futuro modelo de seguro agrícola indexado.

---

## 2. Objetivo del Proyecto

Construir un conjunto de datos climáticos, productivos y económicos que permita:

- Analizar el comportamiento histórico de la precipitación en zonas cafeteras representativas.
- Estimar la relación entre excesos de lluvia y la probabilidad de pérdida productiva.
- Identificar rezagos entre eventos climáticos y afectaciones en cosecha.
- Explorar la relación entre oferta/demanda y precio del café como proxy de pérdidas.
- Sentar las bases para un modelo predictivo que soporte un seguro indexado.

---

## 3. Selección de Departamentos y Municipios

Para obtener una muestra representativa y manejable, seleccionamos cinco departamentos líderes en producción de café, con un municipio representativo por cada uno.

| Departamento | Municipio | Latitud | Longitud |
|-------------|-----------|---------|----------|
| Antioquia | Fredonia | 5.96 | -75.13 |
| Huila | San Agustín | 1.87 | -76.26 |
| Tolima | Ibagué | 4.36 | -75.07 |
| Caldas | Chinchiná | 4.99 | -75.60 |
| Cauca | Suárez | 2.95 | -76.69 |

**Justificación:**  
Estos municipios son zonas históricamente cafeteras, con alta presencia de cultivos permanentes, variabilidad climática significativa y accesibilidad a datos satelitales confiables.

---

## 4. Variables Seleccionadas y Fuentes de Datos

### 4.1 Variables climáticas
- Precipitación diaria (NASA POWER, ERA5, CHIRPS)
- Temperatura mínima y máxima (NASA POWER)
- Índices satelitales NDVI/EVI (MODIS)
- Días de lluvia extrema (>20mm, >30mm, >50mm)

### 4.2 Variables económicas
- Precio interno del café (FNC)
- Precio internacional del café (OIC)
- Diferencial entre ambos precios

---

## 5. Metodología de Extracción y Preprocesamiento

### 5.1 Extracción de datos
- Descarga mediante API de NASA POWER.
- Descarga de ERA5 desde Copernicus.
- Descarga de CHIRPS (precipitación diaria global).
- Extracción de NDVI/EVI desde MODIS.
- Importación de series económicas de FNC y OIC.

#### 5.1.1 Variables 
#### - NASA POWER 

- PRECTOTCORR           MERRA-2 Precipitation Corrected (mm/day) 
- IMERG_PRECTOT         MERRA-2 Total Precipitation (mm/day) 
- T2M                   MERRA-2 Temperature at 2 Meters (C) 
- T2M_MAX               MERRA-2 Temperature at 2 Meters Maximum (C) 
- T2M_MIN               MERRA-2 Temperature at 2 Meters Minimum (C) 
- RH2M                  MERRA-2 Relative Humidity at 2 Meters (%) 
- QV2M                  MERRA-2 Specific Humidity at 2 Meters (g/kg) 
- GWETTOP               MERRA-2 Surface Soil Wetness (1) 
- GWETROOT              MERRA-2 Root Zone Soil Wetness (1) 
- ALLSKY_SFC_SW_DWN     CERES SYN1deg All Sky Surface Shortwave Downward Irradiance (MJ/m^2/day) 
- WS2M                  MERRA-2 Wind Speed at 2 Meters (m/s) 

### 5.2 Unificación temporal
Los datos serán transformados en:
- Series semanales.
- Series mensuales.

### 5.3 Incorporación de rezagos
Debido al ciclo productivo del café (7–9 meses entre floración y cosecha), se incorporarán rezagos:
- 1 semana  
- 1 mes  
- 6 meses  
- 7–9 meses (floración → impacto en cosecha)

### 5.4 Limpieza de datos
- Imputación de faltantes.
- Control de calidad entre fuentes climáticas.
- Detección de valores extremos.
- Homogeneización temporal y espacial.

### 5.5 Construcción de indicadores derivados
- Precipitación acumulada semanal/mensual.
- Número de días de lluvia extrema.
- SPI (Standardized Precipitation Index).
- Tendencia NDVI semanal/mensual.
- Variación relativa del precio interno del café.

---

## 6. Fundamento Agronómico del Café

### 6.1 Duración del ciclo del café
- Una cosecha completa dura entre **7 y 9 meses** desde la floración.
- Colombia tiene dos periodos de cosecha:
  - Cosecha principal: septiembre a diciembre.
  - Mitaca: abril a junio.

### 6.2 Períodos más vulnerables al clima
| Etapa | Vulnerabilidad | Impacto |
|-------|----------------|---------|
| Floración | Muy alta | Caída de flores, pérdidas futuras aseguradas. |
| Cuajado del fruto | Alta | Granos pequeños o deformes. |
| Pre-cosecha | Alta | Caída de frutos, sobrehumedad. |

Esto justifica el análisis de rezagos y la identificación de **puntos críticos de precipitación**.

---

## 7. Estructura del Repositorio


---

## 8. Preguntas Analíticas Clave

- ¿Existe un umbral de precipitación a partir del cual aumenta la probabilidad de pérdida?
- ¿Qué rezago de precipitación afecta más a la producción?
- ¿Cómo se relaciona NDVI con eventos de estrés hídrico?
- ¿Cómo evoluciona el precio del café durante periodos de lluvia extrema?
- ¿Es posible construir un índice climático confiable y replicable para seguros?

---

## 9. Próximos Pasos

1. Descargar datos climáticos para los cinco municipios seleccionados.  
2. Construir las series temporales semanales y mensuales.  
3. Realizar análisis exploratorio de correlaciones y tendencias.  
4. Identificar umbrales críticos de precipitación asociados a pérdidas.  
5. Evaluar modelos iniciales predictivos basados en clima y NDVI.  
6. Preparar los insumos para la fase de modelado (Guías 5 y 6).  

---

## 10. Conclusión

Este repositorio constituye la base metodológica y técnica para construir un modelo analítico que permita relacionar la precipitación climática con el riesgo productivo en café.  
El objetivo final es avanzar hacia un **modelo de seguro indexado replicable, transparente y escalable**, alineado con la realidad de los caficultores colombianos.

---