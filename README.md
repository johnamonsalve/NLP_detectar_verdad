# 2024 Modelo NLP para detección de la veracidad o no de mensajes del juego DIPLOMACY mediante XLNet-model
Modelo de predicción a partir de cadena de caracteres que indica si la frase es verdadera o falsa, utilizando el modelo pre-entrenado XLNet. https://huggingface.co/docs/transformers/model_doc/xlnet

# Fuentes y modelo QANTA Projec DIPLOMACY [punto de patida del ejercicio]
VIDEO: https://www.youtube.com/watch?v=BVAAhIUtf9U
PROYECTO: go.umd.edu/diplomacy_data
CODIGO: https://github.com/DenisPeskov/2020_acl_diplomacy
TOOLKIT: https://convokit.cornell.edu/documentation/diplomacy.html
PAPER: https://users.umiacs.umd.edu/~jbg/docs/2020_acl_diplomacy.pdf

# Análisis del proyecto previo con el modelo AllenNLP
Se descarga e instala el repositorio de Github llamado -2020_ACL_DIPLOMACY-MASTER,
El cual requiere la versión Python 3.7.16 en la ruta local del usuario windows. [No ejecuta fuera de ella].
Adicional, este codgo genera incompatibilidad con la librerias urllib3 y overrides;
a las cuales deben realizarse un downgrade para que el modelo pueda ejecutarse.

Ejemplo:
pip install overrides==3.1.0

Successfully built overrides
Installing collected packages: overrides
  Found existing installation: overrides 7.7.0
    Uninstalling overrides-7.7.0:
      Successfully uninstalled overrides-7.7.0
Successfully installed overrides-3.1.0

Luego de instaladas la librerias Allennlp y demás requeridas, 
este modelo arroja con los diferentes escenarios que propone [Macro F1 y Lie F1]
las siguientes métricas:

PS C:\2020_acl_diplomacy-master> allennlp train -f --include-package diplomacy -s 
logdir configs/actual_lie/lstm.jsonnet

2024-01-28 00:31:17,719 - INFO - pytorch_pretrained_bert.modeling:
# Human baseline, macro: 0.5814484420580899
# Human baseline, lie F1: 0.22580645161290322
# Overall Accuracy is,  0.8836363636363637

# Modelo propuesto: XLNet
Modelo de predicción a partir de cadena de caracteres que indica si la frase es verdadera o falsa, utilizando el modelo pre-entrenado XLNet. https://huggingface.co/docs/transformers/model_doc/xlnet

Este modelo [XLNetForSequenceClassification], es un modelo preentrenado que se espera que sea afinado en un conjunto de datos específico antes de realizar predicciones en un tema particular.

En este caso el modelo no ha sido entrenado para tareas específicas y por lo tanto, su capacidad para realizar predicciones con precisión en nuevos datos es limitada. El objeivo de este ejecicio  era llegar a un nivel 
de madurez incipiente para lueg afinarlo al conjunto de datos propuesto.

El conjunto de datos se extrae de un archivo JSON con 17.289 interacciones, 83 speakers y 246 conversaciones,
las cuales se convirtieron a un dataframe con la siguiente estructura de metadata:
"id"
"root"
"text"
"speaker"
"meta"
"speaker_intention"
"receiver_perception"
"receiver"
"absolute_message_index"
"relative_message_index"
"year"
"game_score"
"game_score_delta"
"deception_quadrant"
"reply-to"
"timestamp"

De esta información se tomanlos campos "text" como el mensaje a entrenar y "speaker_intention" como el resultado
[Falso o Verdadero] del mensaje.

Se entrena el modelo de datos con una división 80-20 en test y pruebas.

# Análisis de resultados
Desafortunadamente no puede llegarse a una comparación entre los dos modelos [AllenNLP y XLNet], 
dado que no se obtuvo metricas desde el modelo XLNet por las siguientes razones:

*** Se presentaron problemas de rendimiento en la máquina que se desarrolló el modelo que hacía volver a iniciar y en ocasiones reinstalar los componentes y librerías.

*** Se generaron bucles infinitos en el número de veces que se pasaba por el conjunto de entrenamiento.

# Recomendaciones

-Afinamiento (Fine-Tuning): Continuar el entrenamineto del modelo bajo el conjunto de datos específico. 
-Modificar el código para incluir un bucle de entrenamiento en los datos antes de realizar la evaluación. 
-Adaptar la función de pérdida y otros hiperparámetros según sea necesario.
-Evitar el sobreentremiento o bajo entrenamiento como se vayan presentando las mediciones de accuracy
-Guardar el Modelo después de afinar para su uso posterior sin tener que volver a entrenarlo cada vez.

# Notas finales

Como anotación personal este proyecto me permitió conocer y explorar varios modelos [como BERT y GPT],
que tambien pueden ser aplicables a esta clase de necesidades. Aunque no logré el resultado solicitado
en el tiempo acordado, fue muy enriquecedor para mi enfrentarme a códigos y repositorios externos para
entender y ampliar mi conociiento frente a esta clase de modelos. Considero que al retarnos con esta clase
de proyectos mejoramos las habilidades y skills para nuevas oportunidades. Gracias. Jhon A. Monsalve. 

