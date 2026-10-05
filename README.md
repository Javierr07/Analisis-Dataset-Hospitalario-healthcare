# Análisis de Dataset Hospitalario healthcare

## 1. Descripción
Usando el Dataset healthcare realizaremos un analisis completo y mas complejo en lo que es la transformacion de datos y modelado.

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

### 1.2 Manejo de Duplicados.
en la exploracion de los datos encontramos que muchos registros son duplicados y que muchos de ellos la unica diferencia es la Edad del paciente, de esta forma llegmos a la conclusion que teniamos datos diplicados y datos duplicados que nos dan inconsistencia en la edad del paciente, el resultado fue que 534 registros estan duplicados exactamnete igual y que 4,436 registros eran duplicados y su inica diferencia es la edad, dandonos un total de 5,000 registros duplicados.

La imagen siguiente es una muestra de como los registros eran duplicados a escepcion de la edad.

La forma en la que tratamos estos datos fue filtrando los datos duplicados a 1 registro y los que tenian diferencia en la edad se utilizo solo un registro y debido a que no se tenia certeza en la edad decidimos dejarla null y esperar una correcion en el daro erroneo.
<img width="1118" height="188" alt="image" src="https://github.com/user-attachments/assets/1c6a669e-8afc-4a58-a464-eed43ad9bc63" />

## 2. Modelado de Datos

En la investigacion para el modela de datos descubrimos que par aun total de 50000 registros de hospitalizaciones los hospitales que se utilizaron fueron 39876 por lo que este hallazgo nos limita a realizar analisis mas detallados con respecto a los hospitales ya que los registros estan muy dispersos en la cantidad de hopitales rewgistrados.

<img width="515" height="246" alt="image" src="https://github.com/user-attachments/assets/233b3c1c-0b0d-4eed-8ded-2d9cde2b41fd" />


Realizamos el modelado estrella usando la tabla de hechos FactHospitalizacones, y las diferentes dimensiones y sus respectivas relaciones lo que nos ayudara a tener un analisis mas optimizado ordenado, y agregamos la Dimension Fecha la cual nos ayudara a tener un analisis mas preciso con respecto al tiempo.
<img width="808" height="605" alt="image" src="https://github.com/user-attachments/assets/4028a6e2-c261-4e28-b94d-9d0ae7892141" />

## 3. Definición de métricas con DAX.
las KPIs utilizadas para el analisis son las siguientes.

- Total de Hospitalizacion.
  cuenta el total de los registros.

  Total Hospitalizaciones = COUNTROWS(FactHospitalizaciones)
  
- Total de Facturacion.
  Suma la facturacion de cada registro.

  Facturacion Total = SUM(FactHospitalizaciones[Billing Amount])

  
- Facturacion media.
  se suma el total de la facturacion y se divide entre el total de registros.

  facturacion media = 
    DIVIDE(
    [Facturacion Total],
    [Total Hospitalizaciones])
  
- Dias promedios de estancia.
  Suma los dias de estancia de cada hospitalizacion y los divide entre el total de registros.
  Estancia Media = AVERAGE(FactHospitalizaciones[Dias Estancia])
  
- Grupos de edades.
Categorizamos las edades de los pacientes asi tenemos un mejor analisis.

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

## Construccion del Dashboard.


