# Proyecto Final

## Introducción
El tenis es un deporte internacionalmente conocido y practicado desde hace siglos. Los partidos de la ATP (Asociación de Tenis Profesional) mueven a mucha gente y generan grandes aficiones alrededor de los jugadores y los torneos. El resultado de un partido de tenis muchas veces puede ser inesperado; no depende solo de los jugadores, sino también de la superficie en la que se juegue, del sitio, del torneo, etc.

## Conjunto de datos
El conjunto de datos elegido para el proyecto consiste en múltiples datasets que contienen información de los partidos de tenis de ATP, en categoría masculina e individuales, jugados entre los años 1991 y 2023.
Los datos se han extraído del repositorio de Github tennis_atp, del perfil de Jeff Sackmann. Se puede consultar la fuente a través del siguiente enlace: https://github.com/JeffSackmann/tennis_atp. 
Se han seleccionado los archivos atp_matches, acotando del 1991 al 2023. El motivo es que es a partir del año 1991 que se incluyen los datos relativos a los puntos obtenidos en el partido por cada jugador, como los aces ('w_ace', 'l_ace'), las dobles faltas ('w_df', 'l_df'), etc, y que son necesarios para el entreno del modelo.

## Objetivos del proyecto
Los objetivos del proyecto son:
- limpiar y analizar los datos;
- entrenar un conjunto de modelos de clasificación para que puedan predecir qué jugador gana el partido; y
- evaluar el rendimiento de cada modelo, para posteriormente encontrar los mejores parámetros para mejorarlos. 

También se ha querido comparar el rendimiento del modelo según si se incluían o no los atributos relativos al rendimiento de los jugadores durante el partido (e.g. 'w_ace', 'w_df', etc.). Debe tenerse en cuenta que el modelo no debería entrenarse con información que no estaría disponible en el momento de hacer la predicción. El caso más claro es la variable 'score', que hace referencia al resultado del partido y que directamente nos indica qué jugador ha ganado. En cuanto a las variables que indican el rendimiento de cada jugador, estas no nos permiten saber quien ha ganado, pero son valores que obtendríamos al final del partido. En la práctica, lo ideal sería que estos valores se sustituyeran por datos sobre el rendimiento medio del jugador en los últimos partidos, pero no ha sido posible obtener estos datos. Es por este motivo que se han hecho dos versiones del apartado de ML para ver como cambia el rendimiento de los modelos según si incluimos o no los datos sobre el rendimiento de los jugadores durante el partido.

## Limpieza de los datos ('data_cleaning.ipynb')

En este apartado se han tratado los NaNs. Para ello, se han aplicado técnicas de imputación cuando ha sido posible y tenía sentido (como sustituir el valor nulo por la media del conjunto o la técnica de atribución); se han eliminado las columnas con muchos NaNs; y se han eliminado los registros con NaNs cuando los atributos eran críticos para el modelo y los NaNs representaban una porción muy pequeña del conjunto de datos. También se ha modificado el tipo de dato cuando se ha considerado necesario.

## Análisis exploratorio de los datos (EDA) ('EDA.ipynb')

El análisis incluye: (1) el número de partidos jugados por nivel de torneo, (2) el número de partidos jugados por tipo de superficie, (3) el número de partidos jugados por nivel de torneo y  tipo de superficie, (4) los 10 mejores jugadores por cada tipo de superficie, teniendo en cuenta las victorias de cada jugador en rondas finales, (5) los 10 jugadores que acumulan más victorias, y (6) las nacionalidades más comunes entre los 10 mejores jugadores.

## Machine Learning
Antes de proceder a la transformación de las variables, se ha modificado el nombre de aquellas que incluían alguna referencia al concepto de 'Winner' o 'Loser', ya que estarían indicando directamente al modelo quien gana el partido. Para ello, se ha asignado a los jugadores el número 1 o 2 de forma aleatoria, obteniendo las variables 'Player1' y 'Player2'.
También se ha creado un 'label' (o etiqueta), siendo 1 cuando el jugador 1 gana el partido y 0 cuando gana el jugador 2.

Seguidamente, se ha estudiado la correlación entre cada variable y el 'label' y se han eliminado los atributos con menor correlación con el label y aquellas variables categóricas que no eran críticas para la predicción. En la segunda versión se han eliminado también todas las variables relativas al rendimiento de los jugadores durante el partido ('w_ace', 'w_df', etc.), independientemente de su correlación el el 'label'.

Hecho lo anterior, se han creado las variables X e y se ha aplicado un preprocesado a la variable X mediante un Pipeline. Para las numéricas se ha aplicado el RobustScaler(), dado que no eran normales y no tenían outliers, y para las categóricas se ha aplicado el OneHotEncoder().

Con ello, se ha obtenido un dataset con 94344 filas y 51 columnas en el primer caso, y 94344 filas y 33 columnas en el segundo.

A partir de aquí, se han dividido los datos para cada variable (X e y) en conjuntos Train y Test y se han entrenado los modelos. Finalmente, se ha mejorado el rendimiento de los modelos con la búsqueda de los mejores parámetros, mediante GridSearchCV().

## Conclusiones y sugerencias de mejora
Se ha podido comprobar que el rendimiento de los modelos es considerablemente mejor si se incluye la información sobre el rendimiento de los jugadores durante el partido. Ello tiene sentido por cuanto estos atributos dan mucha información sobre el tipo y nivel de juego de cada jugador. Como sugerencia de mejora, se podría incluir en el dataset datos sobre el rendimiento medio de los jugadores en los últimos partidos, así el modelo tendría más información para hacer las predicciones y serían datos que estarían disponibles en el momento de hacer la predicción, es decir, antes de que se jugara el partido. 

mejores modelos??
