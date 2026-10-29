# 🛒 Proyecto Canasta SEPA (`sepa-lakehouse-ml`)

> El proyecto se encuentra todavía en desarrollo en un repositorio privado. Para consultas a la documentación o el repositorio completo escriba al autor Emiliano Santis al siguiente mail: emilianodavid.santis@gmail.com

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com/)
[![DuckDB](https://img.shields.io/badge/DuckDB-1.0+-yellow.svg)](https://duckdb.org/)
[![Storage](https://img.shields.io/badge/Storage-Parquet-blue.svg)](https://parquet.apache.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon.tech-blue)](https://neon.tech/)
[![MLOps](https://img.shields.io/badge/MLOps-Model%20Registry-purple.svg)](models/registry/)

Bienvenido al proyecto **Canasta SEPA**, una plataforma integral de Data Lakehouse de grado industrial, procesamiento analítico in-memory vectorizado con **DuckDB**, almacenamiento columnar optimizado en **Parquet**, Clasificación Semántica de Productos mediante Machine Learning / NLP (con Inferencia en Cascada 4 Niveles, Patrón Join-Back y Model Registry) y Serving API de alta velocidad con protección **Read-Only** para el seguimiento y comparación de precios de la Canasta Básica Alimentaria.

---

## 🗺️ Índice de Documentación Técnica (*Doc-as-Code*)

La documentación técnica del sistema está organizada de forma modular dentro del directorio [`docs/`](docs/):

1. 🎯 [**00-vision-del-proyecto.md**](docs/00-vision-del-proyecto.md) — **Visión del Proyecto:** Contexto, problema, solución, objetivos, impacto y criterios de éxito.
2. 📐 [**01-arquitectura-general.md**](docs/01-arquitectura-general.md) — **Visión Global:** Diagrama Mermaid.js interactivo, MLOps Model Registry, Patrón Join-Back y seguridad DW Read-Only.
3. 📦 [**02-ingesta-bronce.md**](docs/02-ingesta-bronce.md) — **Faceta 1:** Microservicio downloader & INDEC parser, fuentes de datos SEPA y almacenamiento crudo en `data/1_bronze/`.
4. ⚡ [**03-lakehouse-plata.md**](docs/03-lakehouse-plata.md) — **Faceta 2:** Procesamiento analítico con DuckDB, sanitización UTF-8, validación de EANs y almacenamiento en Parquet.
5. 🧠 [**04-ml-clasificacion.md**](docs/04-ml-clasificacion.md) — **Faceta 2:** Pipeline ML/NLP en Cascada (N0-N3), Patrón Join-Back, Model Registry manifest ($F1 \ge 0.90$) y Feedback Loop desacoplado.
6. 🐘 [**05-warehouse-oro.md**](docs/05-warehouse-oro.md) — **Faceta 3:** Data Warehouse en PostgreSQL (Neon.tech) con rol **Read-Only** y aislamiento en staging.
7. 🚀 [**06-api-serving.md**](docs/06-api-serving.md) — **Faceta 3:** Endpoints REST FastAPI de solo lectura y recepción de feedback a staging.

---

## 📂 Estructura Definitiva del Proyecto

```
sepa-lakehouse-ml/
├── .github/
│   └── workflows/              # Pipelines de CI/CD (Testing, Linting)
├── config/
│   └── config.yaml             # Parámetros estáticos, URLs del SEPA y rutas
├── data/                       # (Gitignored) Datos del Lakehouse Medallion y Sandbox
│   ├── 0_sandbox/              # (Gitignored) Espacio temporal desechable para prototipado ML y subsets de prueba
│   ├── 1_bronze/               # Particionado raw/ (.zip) y extracted/ (CSVs por comercio) por fecha
│   ├── 2_silver/               # Datasets limpios en formato Parquet (productos_armonizados/)
│   └── 3_gold/                 # Tablas finales listas para servir
├── database/                   # Gestión de la base de datos (Data Warehouse)
│   ├── DDL_POSTGRES.sql        # Script DDL con esquemas Oro, Staging y roles Read-Only
│   └── migrations/             # Futuras actualizaciones del esquema SQL
├── docs/                       # DOCUMENTACIÓN TÉCNICA MODULAR (Doc-as-Code)
│   ├── 00-vision-del-proyecto.md   # Visión del proyecto, objetivos y métricas
│   ├── 01-arquitectura-general.md  # Visión global, diagramas Mermaid.js, Join-Back y MLOps
│   ├── 02-ingesta-bronce.md         # FACETA 1: Downloader, fuentes y raw data
│   ├── 03-lakehouse-plata.md         # FACETA 2: Procesamiento con Parquet y DuckDB
│   ├── 04-ml-clasificacion.md      # FACETA 2: Módulo NLP Cascada, Join-Back, Model Registry & Feedback
│   ├── 05-warehouse-oro.md         # FACETA 3: PostgreSQL DW Read-Only & Staging isolation
│   ├── 06-api-serving.md           # FACETA 3: Endpoints REST para la App Móvil
│   └── pdfs/                       # Anexos técnicos del SEPA, fichas y prompts
├── logs/                       # (Gitignored) Archivos .log de ejecuciones
├── models/                     # Artefactos ML/DL y Model Registry Manifest
│   ├── registry/               # Manifiesto manifest.json y binarios (.pkl, .onnx)
│   └── vectorizers/            # Embeddings/Tokenizadores NLP versionados
├── notebooks/                  # Experimentos y desarrollo exploratorio
│   ├── 01_eda_sepa.ipynb
│   ├── 02_duckdb_transform.ipynb
│   └── 03_validaciones.ipynb
├── scripts/                    # Entrypoints para ejecución batch o Cron
│   └── run_daily_pipeline.py
├── src/                        # CÓDIGO FUENTE MODULAR
│   ├── __init__.py
│   ├── core/                   # Clientes de infraestructura unificados (DB Read-Only, DuckDB)
│   ├── ingesta/                # Microservicio Ingesta Bronce & INDEC Parser
│   ├── etl/                    # Motor DuckDB ETL Plata y Loader Oro
│   ├── ml/                     # Preprocesamiento NLP, entrenamiento F1>=0.90 e inferencia Cascada
│   ├── api/                    # Servidor FastAPI REST Read-Only y feedback staging
│   └── utils/                  # Sistema unificado de logs y utilidades
├── tests/                      # Suite de pruebas automatizadas (unit & integration)
├── .env.example                # Plantilla de variables de entorno
├── .gitignore                  # Exclusiones de Git
├── pyproject.toml              # Única fuente de verdad de dependencias
└── README.md                   # Este archivo (Índice Principal)
```

---

## ⚡ Inicio Rápido

### 1. Clonar el repositorio e instalar dependencias
```bash
# Crear y activar entorno virtual Python
python -m venv .venv
source .venv/bin/executable  # O en Windows: .venv\Scripts\activate

# Instalar dependencias del proyecto
pip install -e .
```

### 2. Configurar variables de entorno
Copiar la plantilla `.env.example` a `.env` y configurar las credenciales de PostgreSQL (Neon.tech):
```bash
cp .env.example .env
```

### 3. Entrenar y Promover Modelo al Model Registry ($F1 \ge 0.90$)
```bash
python -m src.ml.train
```

### 4. Ejecutar el Pipeline Batch Diario (DuckDB + Parquet)
```bash
python scripts/run_daily_pipeline.py
```

### 5. Iniciar la API REST Gateway (Read-Only)
```bash
uvicorn src.api.main:app --reload --port 8000
```
Visitar la documentación interactiva en `http://localhost:8000/docs`.
