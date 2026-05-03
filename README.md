# 📊 Análisis de Clientes – ConnectaTel

ConnectaTel es una empresa de telecomunicaciones en Latinoamérica. Este proyecto analiza el comportamiento de sus clientes durante 2024 con el objetivo de entender cómo usan el servicio, qué perfiles existen y qué oportunidades hay para mejorar la oferta de planes.

El análisis va desde la limpieza de los datos hasta la segmentación de clientes y la generación de conclusiones accionables para el negocio.

---

## 🗂️ Datasets

Se trabajó con tres archivos CSV:

- **`plans.csv`** — los dos planes disponibles (Básico y Premium) con sus precios y límites incluidos
- **`users_latam.csv`** — 4,000 usuarios con información demográfica, plan contratado y fecha de registro
- **`usage.csv`** — 40,000 registros de actividad: llamadas y mensajes de texto por usuario

---

## 🧩 ¿Qué se hizo en este análisis?

### 1. Carga y exploración inicial
Se cargaron los tres datasets y se revisó su estructura, tipos de datos y primeras filas para entender con qué se estaba trabajando.

### 2. Identificación de problemas de calidad
- Valores nulos por columna (cantidad y proporción)
- Valores sentinel: `-999` en `age` y `"?"` en `city`
- Fechas mal registradas: 40 usuarios con año de registro 2026

### 3. Limpieza de datos
- El `-999` en `age` se reemplazó con la mediana real (excluyendo ese valor del cálculo)
- El `"?"` en `city` se convirtió en `pd.NA`
- Las fechas futuras en `reg_date` se marcaron como nulas
- Los nulos en `duration` y `length` se conservaron — no son errores, son consecuencia del tipo de evento (llamada vs mensaje)

### 4. Construcción del perfil de usuario
Se creó la tabla `user_profile` combinando los datos de usuarios con métricas de uso agregadas por persona:
- Total de mensajes enviados
- Total de llamadas realizadas
- Total de minutos de llamada

### 5. Visualización y detección de outliers
- Histogramas por variable con separación por plan
- Boxplots para identificar valores extremos
- Cálculo de límites IQR — los outliers se conservaron por representar comportamiento real

### 6. Segmentación de clientes
- **Por nivel de uso:** Bajo uso / Uso medio / Alto uso
- **Por edad:** Joven / Adulto / Adulto Mayor

### 7. Insight ejecutivo
Traducción de los hallazgos en conclusiones y recomendaciones concretas para el negocio.

---

## 📈 Principales hallazgos

- La mayor parte de los clientes pertenece al segmento de **Uso medio**.
- Existe un grupo pequeño de usuarios intensivos con alto potencial de valor.
- El plan Premium no siempre está asociado a un mayor nivel de consumo.
- Hay usuarios del plan Básico con consumo elevado y clientes Premium con uso bajo.
- La edad no parece ser el factor principal en la elección del plan.

---

## ▶️ Cómo ejecutar el notebook

### En Google Colab (recomendado)

1. Abre [Google Colab](https://colab.research.google.com)
2. Ve a **Archivo → Subir notebook** y carga el archivo `.ipynb`
3. Sube los tres datasets desde el panel de archivos (ícono de carpeta)
4. Si es necesario, ajusta las rutas de carga:
```python
plans = pd.read_csv('plans.csv')
users = pd.read_csv('users_latam.csv')
usage = pd.read_csv('usage.csv')
```
5. Ejecuta todas las celdas con **Runtime → Run all**

### En Jupyter local

```bash
pip install pandas seaborn matplotlib
jupyter notebook
```

Coloca los datasets en la carpeta `/datasets/` antes de ejecutar.

---

## ⚠️ Importante

Las celdas deben ejecutarse **en orden**. Varios pasos dependen de variables creadas en pasos anteriores — especialmente `user_profile`, que es la tabla base para todo el análisis desde el Paso 4 en adelante.

---

## 🛠️ Librerías utilizadas

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```

---

## 👤 Sobre este proyecto

Desarrollado como parte del **Sprint 7 – Análisis estadístico para detectar patrones** del programa de Analista de Datos de **TripleTen LATAM**.
