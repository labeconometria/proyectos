# Proyecto 5

1. Descargue el dataset [New York City Airbnb Data](https://www.kaggle.com/dgomonov/new-york-city-airbnb-open-data).
2. Realice una exploración inicial de los datos:
  - ¿Cómo se ven los primeros diez registros?
  - ¿Cómo se ven los últimos diez registros?
  - ¿Qué tamaño tiene la matriz?
  - ¿Cuáles son los nombres de las columnas? Asegúrese de que de aquí en adelante los nombres de las columnas empleen snake_case. Modifique el tipo de datos de las columnas si lo considera necesario.
  - Elimine columnas si lo considera necesario.
  - Cree columnas con nueva información si lo considera necesario.
  - Utilice un método para imprimir un resumen conciso de la información del dataset.
3. **Missing values**: los datos faltantes son un problema común en los datasets de la vida real.
  - Identifique cuántos hay por columnas. 
  - Identifique cuántos hay por filas.
  - Proponga una solución a partir de su criterio y ejecútela.
  - **NOTA**: recuerde que los missing values pueden venir de muchas formas, no solo del tipo `np.nan`. Emplee el siguiente código para una ilustración gráfica
  ```python
  !pip install missingno
  import missingno as msno
  msno.matrix(df) #df es el dataframe importado inicialmente
  ```
4. **Outliers**: identifique si existen outliers univariados o multivariados. Proponga una solución y ejecútela.
  ```python
  from scipy.spatial.distance import mahalanobis
  from scipy.stats import chi2
  
  mahal_distances = []
  for idx, _ in enumerate(df):
    mahal_distances.append(mahalanobis(df.iloc[idx], np.mean(df.iloc[idx]), df.cov()))
  
  df['mahal_distances'] = mahal_distances
  df['p_value'] = 1 - chi2.cdf(df['mahal_distances'], k-1) #donde k es el número de variables
  
  # Si el 'p_value' del registro está por debajo del nivel de confianza (usualmente el 1%), entonces se puede decir que es un outlier
  ```
5. **Datos inconsistentes**: en ocasiones los datos en las columnas no guardan relación con el resto. Esto puede deberse a:
  - Un error de digitación
  - Dato default si no hay respuesta de quien recolectó los datos
6. **Datos duplicados**: estos pueden ser de dos formas.
  - Explícitos: esto sucede cuando los registros son exactamente iguales.
  - Implícitos: esto sucede cuando los valores en las columnas están escritos de manera diferente pero son el mismo dato. Por ejemplo, si una columna indica el número de hijos de una familia y se encuentran las respuestas `4`, `4.0` o `cuatro`, Las respuestas son las mismas pero el dataframe las identifica como tres respuestas diferentes.
7. Realice una visualización univariada, bivariada y multivariada teniendo en cuenta el tipo de variables y el objetivo de la visualización.
