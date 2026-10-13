# Análisis de Dataset Hospitalario healthcare

## 1. Descripción
Seleccionamos el Dataset healthcare para realizar un analisis desde cero usando solo power BI como herramienta de trabajo, el enfoque es utilizar para limpieza, trasformacion e investigacion de datos Power query, Realizar el modelado de ser nesesario y mostrar los resultados de la investigacion y datos relevantes mediante los graficos BI.

## 2. Objetivo
Analizar los registros de hospitalización para identificar patrones en los costes médicos, la duración de las estancias y la distribución de los ingresos hospitalarios, con el propósito de aportar información útil para la planificación operativa.

## 3. Herramientas
- Power Query: limpieza y transformación.
- Power BI y DAX: modelado, métricas y visualización.

## 4. Fuente de Datos
Kaggle
https://www.kaggle.com/datasets/prasad22/healthcare-dataset

## 5. Metodologia

## 1. Limpieza y Transformacion de datos con Power Query.

### 1.1 Tipos de Datos
Correjimos los tipos de datos que power query de esta forma tenemos calculos mas precisos al momento de analizar.
<img width="1250" height="628" alt="image" src="https://github.com/user-attachments/assets/a88270e4-5d79-4d29-a7cc-66a50b53466e" />

### 1.2 Manejo de Duplicados

Durante la busqueda de datos duplicados encontramos algo inreresante.
- 534 registros duplicados.
- 4,936 registros duplicados con una inconsistencia la cual es la edad del paciente.
- Total de 5000 registros duplicados.
- ¿Como manejamos este problema? principalmente filtramos los datos que son exactamente iguales y los que tenian edad diferente debido a que no se sabe cual edad es la correcta se decidio dejar la edad como nulo asi en el futuro de puede correjir con la edad correcta.

La imagen siguiente es una muestra de como los registros eran duplicados a escepcion de la edad.
<img width="1118" height="188" alt="image" src="https://github.com/user-attachments/assets/1c6a669e-8afc-4a58-a464-eed43ad9bc63" />

## 2. Modelado de Datos

En la investigacion para el modela de datos descubrimos que para un total de 50000 registros de hospitalizaciones los hospitales que se utilizaron fueron 39876 por lo que este hallazgo nos limita a realizar analisis mas detallados con respecto a los hospitales ya que los registros estan muy dispersos en la cantidad de hopitales rewgistrados.

<img width="515" height="246" alt="image" src="https://github.com/user-attachments/assets/233b3c1c-0b0d-4eed-8ded-2d9cde2b41fd" />


Realizamos el modelado estrella definiendo la tabla de hechos FactHospitalizacones, y las diferentes dimensiones y sus respectivas relaciones lo que nos ayudara a tener un analisis mas optimizado y ordenado, y agregamos la Dimension Fecha la cual nos ayudara a tener un analisis mas preciso con respecto al tiempo.
<img width="808" height="605" alt="image" src="https://github.com/user-attachments/assets/4028a6e2-c261-4e28-b94d-9d0ae7892141" />

## 3. Definición de métricas con DAX
las KPIs utilizadas para el analisis son las siguientes.

- Total de Hospitalizacion.
  cuenta el total de los registros.

```DAX
Total Hospitalizaciones =
COUNTROWS(FactHospitalizaciones)
```
  
- Total de Facturacion.
  Suma la facturacion de cada registro.
```DAX
Facturacion Total = SUM(FactHospitalizaciones[Billing Amount])
```
  
- Facturacion media.
  se suma el total de la facturacion y se divide entre el total de registros.
```DAX
facturacion media = 
    DIVIDE(
    [Facturacion Total],
    [Total Hospitalizaciones])
```
  
- Dias promedios de estancia.
  Suma los dias de estancia de cada hospitalizacion y los divide entre el total de registros.

```DAX
Estancia Media = AVERAGE(FactHospitalizaciones[Dias Estancia])
```
  
- Grupos de edades.
Categorizamos las edades de los pacientes.

```DAX
Grupo Edad = 
SWITCH(
    TRUE(),
    ISBLANK(FactHospitalizaciones[Age]), "Edad desconocida",
    FactHospitalizaciones[Age] < 18, "Niños y adolescentes",
    FactHospitalizaciones[Age] < 40, "Adultos jóvenes",
    FactHospitalizaciones[Age] < 65, "Adultos",
    FactHospitalizaciones[Age] >= 65, "Adultos mayores",
    "Edad desconocida"
)
```

## 4. Construccion del Dashboard
El dashboard siguiete muestra los siguientes componentes.
- Tabla en la que se muestra la reparticion de las hospitalizaciones que tienen desde 1 a 30 dias de estancia y la suma de la facturacion de todas las hospitalizaciones que tuvo esa estancia.
- KPIs Facturacion total, Total hopitalizaciones, Facturacion media, Estancia media.
- Grafico de anillos el cual muestra la distribucion de los grupos de edad en el cual no tenemos los datos precisos debido a que el 10% de las hospitalizaciones no tienen edad y se han categorizado como edad desconocida.
- Grafico circular en el que vemos la distribucion de las hospitalizaciones por tipo de admision.
- Grafico de linea que muestra la tendencia de las hospitalizaciones en cada años.

<img width="1059" height="621" alt="image" src="https://github.com/user-attachments/assets/cbef1c9b-9af2-4473-a752-9b117e6081a4" />



## 5. Hallazgos

### 5.1 Resultados del análisis

Después del proceso de limpieza y consolidación, el conjunto utilizado para el análisis contiene 50,000 hospitalizaciones.
Los principales indicadores obtenidos fueron:
- 50,000 hospitalizaciones.
- 4966 registros con edad desconocida y configurada como null.
- Total de $1.278 mil millones en importe facturado.
- Facturación media cercana a $25.56 mil por hospitalización.
- Estancia media de 15.5 días.
- Los años 2019 y 2024 tienen menos registros ya que solo hay datos parciales del año.

### 5.2 Hallasgoz Principales

Las hospitalizaciones presentan una distribución altamente uniforme entre las principales variables categóricas analizadas. No se identificaron concentraciones relevantes por tipo de admisión, condición médica, proveedor de seguros, género, medicamento o tipo de sangre.
Al realizar segmentaciones adicionales entre estas variables, las proporciones se mantuvieron similares, sin observarse diferencias suficientemente grandes como para considerarlas patrones relevantes.

La distribución por grupos de edad mostró aproximadamente 33 % de adultos, 29 % de adultos jóvenes y 27 % de adultos mayores. Cerca del 10 % quedó clasificado como edad desconocida debido a inconsistencias detectadas durante la limpieza.

### 5.3 Limitaciones
El conjunto de datos utilizado es sintético y presenta distribuciones altamente uniformes entre numerosas variables. Por esta razón, no se identificaron relaciones suficientemente fuertes como para formular conclusiones sobre comportamientos hospitalarios reales.
Además, el dataset no dispone de un identificador único de paciente o encuentro, por lo que los registros fueron interpretados como hospitalizaciones y no como pacientes únicos.

### Conclusion

El análisis exploratorio no identificó diferencias relevantes entre las principales categorías del conjunto de datos. Las hospitalizaciones permanecieron ampliamente equilibradas al segmentarlas por tipo de admisión, condición médica, proveedor de seguros, género, medicamento y otras características.
Las métricas de facturación y duración de estancia también mostraron comportamientos similares entre los segmentos analizados. El análisis estadístico de la estancia reveló además una distribución aproximadamente uniforme entre 1 y 30 días.
Estos resultados sugieren que la uniformidad observada está relacionada con la naturaleza sintética del conjunto de datos, por lo que no existe evidencia suficiente para plantear recomendaciones operativas o clínicas basadas en diferencias entre grupos.
El principal valor del proyecto se encuentra, por tanto, en el proceso analítico completo: detección y tratamiento de inconsistencias, transformación de datos, modelado dimensional, creación de métricas, análisis exploratorio y validación de posibles patrones antes de formular conclusiones.

## 6. Retos
en la construccion del este proyecto de analisis se presentaron retos los cuales me ayudaron mucho a mejorar mi analisis y tambien mi habilidad tecnica.
- Investigacion para encontrar los datos duplicados y en manejo correcto de estos datos.
- Crear el modelado estrella a partir de una tabla como lo fue este dataset.
- Crear las dimensiones y las diferentes relaciones entre las dimensiones y la tabla de hechos.
- las KPIs que nos ayuden a tener analisis mas preciso.
- Investigacion que respalde las conclusiones.


## 7. Dashboard
Archivo del proyecto en Power BI
[Analisis healthcare.zip](https://github.com/user-attachments/files/33070506/Analisis.healthcare.zip)


