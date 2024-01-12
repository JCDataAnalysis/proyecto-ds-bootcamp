# Proyecto-Final
Data Science Bootcamp

## Introducción 
El tenis es un deporte internacionalmente conocido y practicado desde hace siglos. Los partidos de la ATP (Asociación de Tenis Profesional) mueven a mucha gente y generan grandes aficiones alrededor de los jugadores y los torneos. El resultado de un partido de tenis muchas veces puede ser inesperado; no depende solo de los jugadores, sino también de la superficie en la que se juegue, del sitio, del torneo, etc.

## Objetivos del proyecto
El objetivo general del proyecto es crear un modelo de clasificación que permita predecir el ganador de un partido de tenis de ATP. 

Los objetivos específicos para conseguirlo son:
-	limpiar y analizar el conjunto de datos;
-	eliminar del dataset cualquier referencia a ‘Winner’ y ‘Loser’, ya que implicaría dar directamente la respuesta al modelo. Para ello, otro objetivo es asignar aleatoriamente a cada jugador el número 1 o 2 y crear una variable ‘target’ con los valores 0 y 1, siendo 1 cuando el jugador 1 gane el partido y 0 cuando gane el jugador 2;
-	entrenar un conjunto de modelos de clasificación para que puedan predecir qué jugador gana el partido; 
-	evaluar el rendimiento de cada modelo, para posteriormente encontrar los parámetros del mejor modelo; y 
-	hacer predicciones con los modelos.
  
## Conjunto de datos
El conjunto de datos elegido para el proyecto consiste en múltiples datasets que contienen información de los partidos de tenis de ATP, en categoría masculina e individuales, jugados entre los años 1991 y 2023.

Los datos se han extraído del repositorio de Github tennis_atp, del perfil de Jeff Sackmann. Se puede consultar la fuente a través del siguiente enlace: https://github.com/JeffSackmann/tennis_atp. 

Se han seleccionado los archivos atp_matches, acotando del 1991 al 2023. El motivo es que es a partir del año 1991 que se incluyen los datos relativos a los puntos obtenidos en el partido por cada jugador, como los aces ('w_ace', 'l_ace'), las dobles faltas ('w_df', 'l_df'), etc, y que se utilizarán para el entreno de los modelos.

## Limpieza de los datos ('data_cleaning.ipynb')

En este apartado se han tratado los NaNs. Para ello, se han aplicado técnicas de imputación o población cuando ha sido posible y tenía sentido (como sustituir el valor nulo por la media del conjunto, o asignar el valor anterior de un mismo grupo); se han eliminado las columnas con muchos NaNs; y se han eliminado los registros con NaNs cuando los atributos eran críticos para el modelo y los NaNs representaban una porción muy pequeña del conjunto de datos. También se ha modificado el tipo de dato cuando se ha considerado necesario.

## Análisis exploratorio de los datos (EDA) ('EDA.ipynb')

El análisis incluye: (1) el número de partidos jugados por nivel de torneo, (2) el número de partidos jugados por tipo de superficie, (3) el número de partidos jugados por nivel de torneo y  tipo de superficie, (4) los 10 mejores jugadores por cada tipo de superficie, teniendo en cuenta las victorias de cada jugador en rondas finales, (5) los 10 jugadores que acumulan más victorias, (6) la evolución de los 5 jugadores con más victorias, y (7) las nacionalidades más comunes entre los jugadores con más victorias.

## Machine Learning ('ML_v1.ipynb', 'ML_v2.ipynb', 'app.py')
Antes de proceder a la transformación de las variables, se ha modificado el nombre de aquellas que incluían alguna referencia al concepto de 'Winner' o 'Loser', ya que estarían indicando directamente al modelo quien gana el partido. Para ello, se ha asignado aleatoriamente a cada jugador el número 1 o 2 y se ha creado una variable ‘target’ con los valores 0 y 1, siendo 1 cuando el jugador 1 gana el partido y 0 cuando gana el jugador 2.

También se han identificado atributos que, de introducirse en el modelo, provocarían un 'data leakage'. Dicho de otra manera, se estarían introduciendo datos que realmente no estarían disponibles en el momento de hacer la predicción. Nos referimos principalmente a la variable 'score', que incluye el resultado del partido, pero también a los atributos que hacen referencia al rendimiento de los jugadores durante el partido (como 'w_ace, w_df, etc.). Teniendo en cuenta que estos últimos representan una parte muy importante de la base de datos se ha optado por hacer dos versiones del conjunto de datos para entrenar los modelos con el objetivo de comparar los resultados. En la práctica, lo ideal sería incluir datos sobre el rendimiento medio de los jugadores en partidos pasados, de esta manera el modelo tendría más información sobre el nivel de juego de los jugadores y no se produciría ningún data leakage. 

A partir de aquí, se han entrenado los modelos de clasificación con las dos versiones de datos y se han tuneado los hiperparámetros de los mejores modelos resultantes con GridSearchCV(). 

Por último, se han hecho predicciones con los modelos.

## Conclusiones y sugerencias de mejora
Se ha podido comprobar que el rendimiento de los modelos es considerablemente mejor si se incluye la información sobre el rendimiento de los jugadores durante el partido. Ello tiene sentido por cuanto estos atributos nos dan mucha información sobre el tipo y nivel de juego de cada jugador. Los mejores modelos para cada una de las versiones han sido: para la primera versión el lr (Logistic Regression), y para la segunda versión el svc (Support Vector Classifier).

Como sugerencia de mejora, se podrían incluir en el dataset datos sobre el rendimiento medio de los jugadores en los últimos partidos, así el modelo tendría más información para hacer las predicciones y serían datos que estarían disponibles en el momento de hacer la predicción, es decir, antes de que se jugara el partido. 
