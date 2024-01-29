by CESAR MALDONADO PARRA
# 2020_acl_diplomacy
Repository for ACL 2020 Paper: "It Takes Two to Lie: One to Lie, and One to Listen"

## Setup

Contenido
1. data_exploration_visualization.ipynb

este archivo incluye un análisis detallado de la data de entrenamiento, esencial para comprender la distribución y características clave de los datos antes de proceder con el desarrollo del modelo. 
Este proceso implica una minuciosa limpieza y examen de la columna 'message', pero debido al objetivo o proyecto a realizar no se tuvo que quitar
las palabras STOP_WORDS dado a que estas permiten identificar patrones y comportamientos relevantes en los mensajes. 
Este paso preliminar es crucial para asegurar que el modelo se construya sobre una base de datos bien entendida y correctamente preparada.

se realizan diferentes graficas como un histograma para el entendimiento de la información, se creo una columna para verificar como estaban distribuidos la data
en cuanto a la frecuencia y el tamaño del mensaje en la columna 'message'



2. data_cleaning_modeloNLP.ipynb

Se encuentra la lectura de los datos de entrenamiento, el desempaquetamiento, la evaluacion de los datos de validacion y de testeo del modelo.
luego se cuenta con la transformacion de la data a un dataframe para poderlo procesar.
se revisa la distribución de clases en el conjunto de entrenamiento se confirma que este 
tiene un DESBALANCEO DE CLASES, por lo anterior se concluye que para equilibrar las clases
se puede poner en equilibrio haciendo un SOBREMUESTREO DE LA CLASE MINORITARIA  duplicando los valores
con la denominacion con la clase 0 .

luego se empieza a crear el modelo, pero antes de eso se define las librerias BERT y luego Pytorch,
inicialmente se tokeniza la data de entrenamiento con BertTokenizer usando si codificador transformer,
su objetivo es generar un modelo de lenguaje entendiendo el analisis del lenguaje humano.

segundo con la data ya tokenizada se procede a Comprobación de las dimensiones de salida para asegurarnos
de que si lo hizo bien, 

A continuacion se crea el #TensorDataset, los tensores serán los input_ids y attention_masks generados por el tokenizador,
luego se crea el #DataLoader que es una clase que proporciona un iterable sobre el conjunto de datos. Con DataLoader,se puede especificar 
el tamaño del lote (batch size), si los datos deben ser mezclados, y otros parámetros que son útiles durante el entrenamiento.
los tokens se convierten en tensores para que puedan ser procesados por los modelos de aprendizaje automatico

 PyTorch es una herramienta poderosa en el campo de procesamiento del lenguaje natural que nos ayuda
a crear y entrenar modelos de aprendizaje profundo.
entonces en conclusion, se utilizo BERT para procesar y entender los datos de texto y luego se utilizo PyTorch
para entrenar un modelo de aprendizaje profundo utilizando estos datos procesados.

#CUDA compatible con PyTorch para que nuestro pc nos reconozca la tarjeta grafica y trabaje por GPU


Entonces el proceso es :

despues de tokenizar y tener los tensores se procede a configura el modelo, 
a cargar el modelo preentrenado de BERT, se define el optimizador y la Función de Pérdida,
se define el # de epocas de entrenamiento y se procede a hacer el bucle para el entrenamiento.

luego de entrenar, el modelo nos entrega una pérdida promedio sobre la época en la que iteramos 
o sea que a medida de que lo iteremos mas veces el modelo va a prender mas como se muestra en los resultados.

Result:
Average training loss: 0.24340122679013246
Average training loss: 0.03561993770310845

podemos visualizar que en la segunda epoca o iteración aprendio mejor este modelo 

luego, para empezar a evaluar el modelo, se procede a evaluar este con los datos de validación
los cuales tambien deben de ser tokenizados y con sus respectivos tensores y pasados a la 
funcion de evaluacion con sklearn, esta nos arroja la matrix de confusión a la cual podemos sacar
y ver los resultados de evaluacion dando un valor de exactitud de un 91%

Ya por ultimo para saber si nuestro modelo no se quedo sobreentrenado (overfitting) o muy rigido
procedemos a meterle nuestros datos como lo son los datos de test, igualmente se tokeniza y se le
saca su respectivo tensor luego esto se lleva a la funcion de evaluacion y finalizamos con la
matrix de confusion dando por enterado que este algoritmo dio un valor de exactitud en un 86%

.......

Recomendacion: 
- Se observa que el modelo tiene una exactitud buena, pero esta mas equilibrado para los valores de mensaje
verdadero o clase 1. debito a esto se recomienda entrenar con mas iteracion el algoritmo para que pueda aprender mas
y se aconseja ajustar los hiperparametros como lo son learning rate o los epochs con mas tiempo 



***************************
******* COMPARACION *******
***************************
A continuacion veremos la comparación de las métricas del modelo resultante con el punto de referencia proporcionado en el documento.

observando las métricas claves:

Modelo ACL20:

Accuracy (Exactitud): 0.71
Precision (Precisión): 0.72
Recall (Recuperación): 0.69
F1-Score: 0.70

Modelo resultante de data_exploration_visualization.ipynb :

Accuracy (Exactitud): 0.86 (evaluación) y 0.91 (validación)
Precision (Precisión): 0.92 (evaluación) y 0.96 (validación) para la clase 1
Recall (Recuperación): 0.93 (evaluación) y 0.94 (validación) para la clase 1

Exactitud: mi modelo tiene una exactitud más alta en comparación con el modelo de referencia, lo que indica un mejor rendimiento general en la clasificación correcta de mensajes.
Precisión y Recall para la Clase 1: mi modelo también muestra una precisión y un recall superiores para la clase 1 (presumiblemente "mentiras") en comparación con el modelo de referencia.

teniendo en cuenta que mi modelo se basa mas en el equilibrio o detecta mas facil los mensajes verdaderos que las mentiras,
sin embargo comparando con el documento podemos aclarar que me dan la razón cuando dicen que 
"una mentira real detectada tanto el modelo como el jugador destinatario estan en lo correcto y que ambos estan equivocados cuando tratan de 
predecir especificamente mentiras" .
Doy asi por concluido que es muy dificil que hasta un ser humano detecte mentiras según esta prueba cientifica.
"Es mas probable que las mentiras contengan sujetividad en el mensaje y detalles sobre las premisas a comparacion de los mensajes
verdaderos que incluyen frases de expansion".



Consideraciones:

La clase minoritaria (presumiblemente "verdades") en el modelo tiene un rendimiento significativamente más bajo, lo que indica una posible área de mejora en el algoritmo por lo tanto no seria una comparación justa entre ambos modelos, ya que mi algoritmo toca ajustarlo con tiempo para el entrenamiento.
En resumen, mi modelo muestra un rendimiento prometedor en comparación con el modelo de referencia, especialmente en la clasificación de la clase mayoritaria. Sin embargo, hay margen para mejorar la precisión en la detección de la clase minoritaria.





## Citation
```
@inproceedings{Peskov:Cheng:Elgohary:Barrow:Danescu-Niculescu-Mizil:Boyd-Graber-2020,
	Title = {It Takes Two to Lie: One to Lie and One to Listen},
	Author = {Denis Peskov and Benny Cheng and Ahmed Elgohary and Joe Barrow and Cristian Danescu-Niculescu-Mizil and Jordan Boyd-Graber},
	Booktitle = {Association for Computational Linguistics},
	Year = {2020},
	Location = {The Cyberverse Simulacrum of Seattle},
}
```

Shield: [![CC BY 4.0][cc-by-shield]][cc-by]

This work is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
