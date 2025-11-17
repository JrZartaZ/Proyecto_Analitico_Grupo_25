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

### 5.1.1 Variables 
#### *NASA POWER*

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


### Variables climáticas seleccionadas (NASA POWER)  
A continuación se presentan las variables seleccionadas del repositorio NASA POWER, junto con su prioridad para el análisis y la justificación científica que soporta su relevancia frente al riesgo agrícola del café.

| **Categoría**       | **Variable (NASA POWER)**                 | **Prioridad** | **Razón científica** |
|---------------------|-------------------------------------------|--------------:|------------------------|
| Precipitación       | Precipitación diaria                      | ⭐⭐⭐⭐⭐ | Principal causa de pérdidas: caída de fruto, hongos, asfixia radicular. |
| Precipitación       | IMERG (alta resolución)                   | ⭐⭐⭐⭐⭐ | Mayor resolución → menor error en zonas montañosas; ideal para café. |
| Temperatura         | T2M (temperatura a 2m)                    | ⭐⭐⭐⭐⭐ | Afecta floración, desarrollo y tasas de fotosíntesis. |
| Temperatura         | T2M_MAX                                   | ⭐⭐⭐⭐ | Determina estrés térmico y marchitez. |
| Temperatura         | T2M_MIN                                   | ⭐⭐⭐⭐ | Riesgo de frío → afecta floración y frutos nuevos. |
| Humedad             | Humedad relativa                          | ⭐⭐⭐⭐ | Relacionada con enfermedades, estrés y hongos. |
| Humedad             | Humedad específica                        | ⭐⭐⭐ | Balance hídrico del cultivo. |
| Suelo               | Humedad superficial (0-5 cm)              | ⭐⭐⭐⭐⭐ | Determina riesgo de hongos, encharcamiento y saturación. |
| Suelo               | Humedad radicular (0-100 cm)              | ⭐⭐⭐⭐⭐ | Impacta productividad → zona donde el café toma nutrientes. |
| Radiación           | Onda corta descendente                    | ⭐⭐⭐ | Relacionada con fotosíntesis y ciclos fenológicos. |
| Viento              | Velocidad del viento                      | ⭐⭐ | Baja correlación con pérdidas, pero útil para estrés evaporativo. |

> **Nota:** Se eligieron variables con fuerte sustento agronómico y evidencia empírica en estudios de café de Cenicafé, FAO y literatura internacional.

---

###  Umbrales climáticos críticos para el cultivo de café
Basados en estudios agroclimáticos, Cenicafé y literatura científica, se definen niveles de riesgo asociados con precipitación y sequía:

| **Fenómeno**                     | **Nivel**   | **Riesgo** |
|----------------------------------|-------------|------------|
| Lluvia diaria > **20–25 mm**     | Alto        | Afecta floración, aumenta caída de flor y riesgo de hongos. |
| Lluvia semanal > **120 mm**      | Muy alto    | Caída de fruto, proliferación de enfermedades. |
| Lluvia mensual > **400 mm**      | Crítico     | Alto riesgo de pérdida de cosecha por saturación y enfermedades. |
| Secas prolongadas > **20 días**  | Crítico     | Reducción en productividad futura, estrés hídrico y pérdida de floración. |

> **Interpretación:**  
> Estos umbrales servirán como **puntos de corte** para clasificar periodos como *“riesgo de pérdida”* vs *“condiciones normales”* para alimentar el modelo analítico.

---

#### *Federación Nacional de Cafeteros (FNC)*

## Argumento técnico para usar el **Precio Interno del Café** (Fuente: FNC)

El **precio interno del café publicado por la Federación Nacional de Cafeteros (FNC)** es la referencia económica más adecuada para este proyecto debido a su relevancia operativa, su solidez metodológica y su alta correlación con factores climáticos que afectan la producción. A continuación, se presentan las razones técnicas que justifican su uso:

---

### 1. Indicador oficial del mercado colombiano
La FNC publica el precio base del café pergamino seco utilizado por cooperativas, compradores y productores.  
Este precio es:

- Oficial y autorizado a nivel nacional.  
- Representativo de las transacciones reales del mercado interno.  
- Usado operacionalmente por los actores de la cadena productiva.

Por lo tanto, permite cuantificar de manera fiel el impacto económico asociado a eventos climáticos adversos.

---

### 2. El precio interno sintetiza múltiples factores económicos
El valor publicado por la FNC integra en un único indicador:

- Cotización internacional del café (ICE – Nueva York).  
- Tasa de cambio COP/USD.  
- Prima del café colombiano por calidad.  
- Descuentos logísticos y comerciales.  
- Costos y dinámicas del mercado interno.

Esto convierte al precio interno en un **indicador compuesto**, ideal para capturar la señal económica total del sector.

---

### 3. Sensibilidad a variaciones en la oferta (por clima)
Eventos como lluvias intensas, sequías o fenómenos ENSO afectan:

- Volumen producido  
- Calidad del grano  
- Flujo de cosecha al mercado  

Cuando ocurre pérdida de producción → **el precio interno tiende a subir**.  
Cuando hay exceso de oferta → **el precio baja**.

Esto lo convierte en una variable proxy perfecta para evaluar **riesgo agroclimático y pérdidas económicas** en el cultivo del café.

---

### 4. Frecuencia diaria y consistencia histórica
El precio interno de la FNC se caracteriza por:

- Disponibilidad diaria  
- Alto historial temporal  
- Metodología estable  
- Ausencia de vacíos significativos  

Estos atributos permiten:

- Resampling semanal/mensual consistente  
- Análisis temporales robustos  
- Unión limpia con series de clima de NASA POWER

Garantiza un análisis técnico confiable.

---

### 5. Fuente verificable y trazable
Al provenir de una entidad oficial y auditada:

- La información es pública, verificable y reproducible.  
- Permite documentar procesos con transparencia.  
- Es adecuada para análisis científicos, regulatorios o académicos.

Su trazabilidad lo convierte en un insumo de alta calidad para proyectos analíticos.

---

### 6. Permite modelar impacto económico
Para un proyecto cuyo objetivo es relacionar:

**Condiciones climáticas ←→ Impacto económico**

…el precio interno es ideal porque refleja:

- Señales inmediatas de estrés productivo.  
- Expectativas del mercado ante riesgos climáticos.  
- Variabilidad económica vinculada al ciclo agrícola.

Ejemplo práctico:

> Lluvias >120 mm semanales en departamentos productores clave reducen la producción, afectan la calidad del grano y generan aumentos en el precio interno debido a menor oferta.

Este comportamiento permite crear:

- Modelos de clasificación (riesgo de pérdida vs normal).  
- Modelos predictivos del precio a partir del clima.  
- Señales de alerta temprana para riesgos agrícolas.

---


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