# Análisis de marca empleadora de BMW con NLP

Proyecto de análisis de marca empleadora a partir de reseñas de empleados en Glassdoor, comparando **BMW Group** con varios competidores del sector automoción mediante técnicas de Procesamiento del Lenguaje Natural.

El proyecto combina **análisis de sentimiento**, **topic modeling** y **benchmarking competitivo** para identificar fortalezas, debilidades y prioridades accionables para Recursos Humanos.

## Objetivo de negocio

El objetivo principal es entender cómo se posiciona BMW Group como empleador y responder a tres preguntas:

- ¿Cuáles son las principales fortalezas de BMW como marca empleadora?
- ¿Qué problemas representan una desventaja frente a sus competidores?
- ¿Qué acciones podría priorizar RRHH para mejorar la atracción y retención de talento?

## Empresas analizadas

### Empresa objetivo

- BMW Group

### Peers

- Mercedes-Benz Group
- Volvo Group
- Jaguar Land Rover
- Tesla

Estas empresas se seleccionaron por pertenecer al sector automoción y competir por perfiles similares en áreas como ingeniería, tecnología, producción, operaciones, diseño y gestión.

## Dataset

El análisis utiliza reseñas de empleados de Glassdoor procedentes de los datasets **Glassdoor Job Reviews** de David Gauthier.

Las variables disponibles incluyen:

- Empresa
- Fecha de la reseña
- Situación del empleado: actual o antiguo
- Rating general
- Recomendación de la empresa
- Aspectos positivos (`pros`)
- Aspectos negativos (`cons`)

El conjunto completo de empresas seleccionadas contiene **10.814 reseñas**.

Para los modelos de NLP se utiliza una muestra equilibrada de **900 reseñas por empresa**, con un total de **4.500 reseñas**. Esta estrategia evita que las empresas con un volumen muy superior de opiniones tengan un peso excesivo en los modelos de topics.

> El dataset completo no se incluye en el repositorio debido a su tamaño.

## Metodología

### 1. Análisis exploratorio de datos

Se analiza:

- Número de reseñas por empresa
- Rating medio
- Porcentaje de recomendación
- Reparto entre empleados actuales y antiguos
- Evolución temporal del rating

### 2. Análisis de sentimiento

Se utiliza el modelo preentrenado:

`distilbert-base-uncased-finetuned-sst-2-english`

El sentimiento se analiza de forma separada para `pros` y `cons` y se utiliza principalmente como **control de calidad**.

Los resultados muestran aproximadamente:

- **93 % de los pros** clasificados como positivos
- **82 % de los cons** clasificados como negativos

Los casos discordantes se revisan manualmente para identificar negaciones, respuestas ambiguas o comentarios cuyo contenido contradice el nombre del campo.

### 3. Topic modeling

Para identificar los temas principales se utiliza **BERTopic**.

Se entrenan dos modelos conjuntos:

- Un modelo para todos los `pros`
- Un modelo para todos los `cons`

Las cinco empresas se incluyen conjuntamente en cada modelo para asegurar que los topics obtenidos sean comparables entre BMW y sus peers.

Antes de seleccionar la configuración final se prueban varias alternativas de BERTopic, comparando:

- Número de topics
- Porcentaje de outliers
- Concentración del topic dominante
- Interpretabilidad desde el punto de vista de negocio

Posteriormente, los topics se agrupan manualmente en categorías interpretables para RRHH:

- Compensación y beneficios
- Conciliación y carga de trabajo
- Cultura y ambiente
- Equipo y liderazgo
- Carrera, aprendizaje y desarrollo
- Procesos y organización
- Marca, producto y propósito
- Condiciones del puesto y operativa
- Inclusión y entorno internacional
- Innovación y entorno dinámico

Los textos que BERTopic no asigna claramente a ningún topic se mantienen como **“Sin tema claro”**, evitando forzar clasificaciones poco fiables.

## Principales resultados

### Posicionamiento general

BMW presenta un rating medio sólido:

- **BMW:** 4,03
- **Media de peers:** ~3,84

Sin embargo, el porcentaje de recomendación es ligeramente inferior al promedio de sus competidores:

- **BMW:** 46,2 %
- **Media de peers:** ~49,1 %

Esto sugiere que la percepción general de BMW como empleador es positiva, aunque existen determinadas fricciones que pueden limitar la disposición de los empleados a recomendar la empresa.

## Principales fortalezas

### Marca, producto y propósito

Es la principal fortaleza diferencial de BMW.

- Balance BMW: **+26,05 puntos porcentuales**
- Balance peers: **+18,27 puntos porcentuales**
- Gap frente a peers: **+7,78 p.p.**

La marca, el producto y el propósito asociado a trabajar en una compañía reconocida del sector automoción constituyen un activo claro para el employer branding.

### Cultura y ambiente

BMW también presenta una posición favorable en esta categoría.

- Balance BMW: **+25,28 p.p.**
- Gap frente a peers: **+1,41 p.p.**

## Principales áreas de mejora

### 1. Conciliación y carga de trabajo

Es la prioridad más clara del análisis.

Representa aproximadamente el **35,2 % de los cons accionables de BMW**.

- Balance BMW: **-26,61 p.p.**
- Balance peers: **-16,86 p.p.**
- Gap frente a peers: **-9,75 p.p.**

Además, esta categoría aparece con mayor peso entre antiguos empleados de BMW y también presenta una desventaja frente a antiguos empleados de los peers.

### 2. Compensación y beneficios

BMW presenta una desventaja relativa en esta dimensión.

- Balance BMW: **-0,69 p.p.**
- Balance peers: **+2,29 p.p.**
- Gap frente a peers: **-2,98 p.p.**

La señal se interpreta principalmente como una posible desventaja de la propuesta de valor al empleado, más que como una causa directa de salida.

### 3. Equipo y liderazgo

BMW se encuentra mejor posicionada que sus peers en esta categoría, pero sigue siendo una señal interna relevante.

Cuando aparece una queja relacionada con liderazgo:

- El rating medio baja aproximadamente a **3,08**
- Solo **27,1 %** de esas reseñas recomienda BMW

Además, este tema aumenta entre antiguos empleados.

Por tanto, se interpreta como una posible **fricción interna relacionada con retención**, aunque BMW se encuentre relativamente mejor que sus competidores.

## Recomendaciones para RRHH

### 1. Mejorar conciliación y carga de trabajo

Identificar los equipos o áreas con mayor concentración de horas extra, picos de carga, presión asociada a turnos y falta de flexibilidad. Posteriormente se podrían implementar pilotos específicos y medir su impacto.

### 2. Revisar competitividad de compensación y beneficios

Realizar benchmarking frente al mercado por perfil profesional, localización y segmento de talento crítico. Los resultados no justifican una subida salarial general, pero sí una revisión dirigida de posibles brechas competitivas.

### 3. Reforzar la calidad del liderazgo

Utilizar información interna como encuestas de clima, feedback 360º y rotación por equipo o manager para identificar áreas en las que los problemas de liderazgo coincidan con baja recomendación o mayor salida de empleados.

## Limitaciones

- Las reseñas de Glassdoor son autoseleccionadas y pueden no representar a toda la plantilla.
- Se combinan distintos países, funciones y unidades de negocio.
- El topic modeling requiere revisión humana para traducir clusters técnicos a categorías de negocio.
- Algunos textos permanecen sin un topic claro.
- Los porcentajes se calculan sobre la muestra analizada.
- Las asociaciones entre topics, ratings y salida de empleados no implican causalidad.
- Los años extremos de la ventana temporal son parciales.

Por tanto, los resultados deberían contrastarse con información interna de RRHH antes de tomar decisiones de alto impacto.

## Estructura del repositorio

```text
.
├── README.md
├── GarciaLinares_Alvaro_NLP_Glassdoor_BMW.ipynb
└── data/
    └── README.md
```

El dataset original no se almacena en el repositorio.

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Hugging Face Transformers
- PyTorch
- BERTopic
- Sentence Transformers
- UMAP
- HDBSCAN
- Scikit-learn

## Reproducibilidad

El notebook está diseñado para ejecutarse de arriba abajo.

La muestra utilizada para NLP se genera con una semilla fija para mantener los resultados reproducibles.

Instalación de las principales dependencias:

```bash
pip install pandas numpy matplotlib scikit-learn pyarrow
pip install torch transformers
pip install bertopic sentence-transformers umap-learn hdbscan
```

## Autor

**Álvaro García Linares**

Máster en Data Science e Inteligencia Artificial  
Formación previa en Marketing y Business Intelligence
