# Visión del Proyecto: SEPA & Canasta Básica Alimentaria (CBA)

## Elección del Data Set
La elección del dataset está orientada a la posibilidad de realizar predicciones y, en parte, brindar la posibilidad de ayudar a dar respuesta a una problemática.
* **Precios SEPA minoristas**
* **Datos Abiertos de Desarrollo Productivo - Precios Claros - Base SEPA**

## Contexto
A través del SEPA (Precios Claros) se maneja un volumen masivo de datos: más de 10 millones de registros de precios minoristas en distintos puntos del país de forma diaria, los cuales se encuentran dispersos y desestructurados.

Por otro lado, la Canasta Básica Alimentaria (CBA) trata de reflejar los hábitos alimenticios de la población argentina por región. Con esto queremos decir que no es ninguna referencia para una canasta alimentaria saludable; el fin primero de la CBA es la medición de la indigencia como un indicador socioeconómico.

De esta manera, basándonos en la Canasta Básica Alimentaria publicada habitualmente de forma mensual por el INDEC (Instituto Nacional de Estadística y Censos), se va a reconstruir dicha canasta con los productos ofrecidos por diferentes supermercados minoristas. La disposición de la información de los precios abre un abanico de posibilidades a la hora de tratarlos, pudiendo adoptar diferentes fines estratégicos variando en criterios y objetivos.

En un escenario donde se busca reflejar precios claros o tener una medida de referencia para el seguimiento de precios minoristas de productos esenciales para el consumo diario, se adopta un enfoque en la CBA basado en los hábitos alimenticios de los ciudadanos.

## Problema
No existe un puente automatizado y estandarizado que relacione la oferta real de los supermercados con la canasta teórica del INDEC. Las descripciones de los productos son caóticas y las unidades de medida varían por sucursal.

## Solución
Construir un pipeline end-to-end automatizado (Data Lakehouse + ML + API) que ingeste, limpie, normalice y clasifique inteligentemente estos datos para reconstruir la CBA y transformarla en información estratégica para la toma de decisiones.

## Propósito
Este proyecto tiene la pretensión de recorrer todo el ciclo de vida que recorren los datos, desde un primer acercamiento como dato crudo hasta el ciclo final transformado en información con propósito especial para la toma de decisiones estratégicas planteadas. De esta manera, el ciclo de vida que seguiremos queda conformado de la siguiente forma:
1. Ingesta y almacenamiento
2. Procesamiento y preparación
3. Exploración
4. Experimentación y predicción (a considerar)

A modo personal podría decirse que la elección del dataset connota ciertas preguntas de carácter implícito que se tratan de aclarar de la siguiente manera. Al parecer se debe aplicar un flujo híbrido, transicionando de un modelo de aprendizaje no supervisado para la clasificación inicial de los productos alimenticios a uno supervisado a definir.

## Objetivos
* **Objetivo general:** Reconstruir la Canasta Básica Alimentaria (CBA) del INDEC utilizando los datos diarios del SEPA.
* **Objetivos específicos:** Implementar un motor analítico basado en DuckDB y almacenamiento en Parquet para limpiar y normalizar las unidades de medida de los productos en la Capa Plata.

## Impacto y Criterios de Éxito

### 1. El Impacto (Valor de Negocio)
* **Democratización y Transparencia de la Canasta Básica:** Permite pasar de depender únicamente de un PDF estático mensual del INDEC a tener una visión dinámica, diaria y granular de cómo los precios reales de los supermercados impactan en la Canasta Básica Alimentaria (CBA) por región.
* **Automatización del Análisis Económico:** Elimina el trabajo manual y artesanal de cruzar millones de registros de precios con categorías oficiales, permitiendo actualizar análisis de inflación y poder de compra en tiempo real.
* **Base Sólida para la Toma de Decisiones:** Proporciona a analistas, investigadores o consumidores una API robusta y rápida para consultar tendencias de precios, comportamiento de marcas y variaciones de la canasta con total trazabilidad.

### 2. Criterios de Éxito (Métricas de Aceptación Técnica)
* **Criterios de Datos y Pipeline (Facetas 1 y 2):**
  * *Idempotencia y Cero Pérdida de Datos:* El pipeline diario de la Capa Bronce debe procesar los paquetes del SEPA de forma automatizada y sin corromper el histórico, logrando un éxito de ingesta continuo del 99.9%.
  * *Escalabilidad y Rendimiento Analítico:* El motor de procesamiento en DuckDB y almacenamiento Parquet (Capa Plata) debe procesar y limpiar los más de 10 millones de registros diarios dentro de una ventana de tiempo razonable y con óptimo uso de memoria local.
  * *Normalización Semántica Exitosa:* Lograr una unificación efectiva de las unidades de medida dispares de los supermercados (gramos, kilos, litros, unidades) a una nomenclatura estándar en al menos el 85% de los productos analizados.
* **Criterios de Machine Learning (Faceta 2):**
  * *Precisión del Clasificador:* El modelo supervisado final (ej. Clasificador de Rocchio / NLP) debe alcanzar una precisión (Accuracy / F1-Score) superior al 80% al emparejar las descripciones libres de los supermercados con las categorías oficiales de la canasta del INDEC.
* **Criterios de Infraestructura y Serving (Faceta 3):**
  * *Latencia de la API:* El API Gateway (FastAPI) conectado a la Capa Oro (PostgreSQL) debe responder a las consultas de la App Móvil con un tiempo de latencia menor a los 50 milisegundos.

## Alcance y Límite del Proyecto
* **En el núcleo actual:** El sistema se enfocará exclusivamente en reconstruir y hacer seguimiento de los productos que componen la Canasta Básica Alimentaria (CBA) oficial del INDEC, utilizando los datos del SEPA.
* **Fuera de alcance (Fase futura):** Queda conceptualmente abierto para futuras iteraciones la incorporación de otros productos esenciales de consumo masivo que no integran la CBA (por ejemplo, productos de higiene o alimentos fuera de la canasta de indigencia), los cuales podrán sumarse una vez estabilizado el modelo principal.

## Base de Datos
* **Modelo de datos:** *(A definir / En construcción)*
* **DER (Diagrama Entidad-Relación):** *(A definir / En construcción)*
