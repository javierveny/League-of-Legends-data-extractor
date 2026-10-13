# League of Legends Match Scraper (Riot API)

This project contains Python scripts and notebooks designed to extract Ranked Solo/DuoQ match data from League of Legends using the official Riot Games API.

The extraction process is set up to mass-download data from a specific patch (defaulting to 16.18, spanning from 10/09/2026 to 22/09/2026) and is divided into two primary approaches:

## Notebook Structure

The project features two distinct Jupyter Notebooks depending on the extraction strategy:

1. **league_dataset_extractor.ipynb** (Original/NOT done): This notebook attempted to iterate through every Elo (Iron to Challenger) to save all PUUIDs and match IDs into .jsonl and .txt files. It is no longer used, as the strict rate limits on Riot's free tier make it impossible to download the entire player base's games effectively.
2. **n_matches_lol_extractor.ipynb** (Current & Recommended Approach): This is the main extractor. It leverages a **Stratified Sampling** method alongside a local **SQLite database** to prevent memory bottlenecks. It performs the following steps:
   - Balances the dataset by collecting an equal number of matches across different Elos (e.g., Emerald, Diamond) using round-robin scheduling.
   - Gathers match IDs played within the target patch's timeframe for SoloQ (Queue 420).
   - Downloads match payloads while automatically filtering out AFK/Remake games (duration under 15 minutes / 900 seconds), saving the clean data to a JSONL file (`matches_dataset.jsonl`).

## Prerequisites

To run this project, you must have [Python](https://www.python.org/downloads/) (version 3.7 or higher) installed.

## Environment Setup and Dependencies

It is highly recommended to use a virtual environment to avoid conflicts with your global system packages. Follow these steps:

### 1. Create the Virtual Environment

Open your terminal in the project folder and run:

**On Windows:**
python -m venv venv
venv\Scripts\activate

**On macOS/Linux:**
python3 -m venv venv
source venv/bin/activate

### 2. Install Dependencies

Once the virtual environment is activated (you will see `(venv)` in your terminal), install the required packages:

pip install -r requirements.txt

### 3. Configure the .env file

The project uses environment variables to keep your Riot API key secure. Create a file named exactly `.env` in the root of your project and add the following line, replacing the value with your actual key:

API_KEY=RGAPI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## Usage

Once everything is configured, you can run the main notebook (`n_matches_lol_extractor.ipynb`). Because it relies on an SQLite database (`riot_dataset.db`), progress is tracked dynamically. This means you can safely pause and resume the extraction without losing data if you hit API limits or close the program.

**Note on API Limits:** If you are using a standard API Key that expires every 24 hours, the scraper will print a warning (401 error) and halt when the key expires. To avoid interruptions, request a non-expiring Production or Personal API Key from Riot at: https://developer.riotgames.com/app-type

---

# League of Legends Match Scraper (Riot API)

Este proyecto contiene scripts y notebooks en Python diseñados para extraer datos de partidas clasificatorias (Solo/DuoQ) de League of Legends utilizando la API oficial de Riot Games.

El proceso está optimizado para obtener datos masivos de un parche específico (por defecto, el 16.18, del 10/09/2026 al 22/09/2026) y está dividido en dos enfoques principales:

## Estructura de Notebooks

El proyecto cuenta con dos Jupyter Notebooks distintos según el enfoque de extracción:

1. **league_dataset_extractor.ipynb** (Enfoque original/NO acabado): Este notebook intentaba recopilar todos los PUUIDs desde Hierro hasta Challenger y guardarlos en archivos .jsonl y .txt. Actualmente no se utiliza, ya que los límites de la capa gratuita de la API de Riot hacen imposible extraer el volumen total de partidas de esta manera.
2. **n_matches_lol_extractor.ipynb** (Enfoque actual y recomendado): Este es el script principal. Utiliza un enfoque de **Muestreo Estratificado (Stratified Sampling)** y una **base de datos local SQLite** para optimizar la memoria RAM y evitar cuellos de botella. Realiza los siguientes pasos:

- Balancea el dataset recopilando una cantidad equitativa de partidas entre diferentes Elos (ej. Esmeralda, Diamante).
- Recopila los IDs de las partidas jugadas en el rango de fechas del parche para la cola SoloQ (Queue 420).
- Descarga la información de la partida, filtrando automáticamente las partidas "Remake" o con jugadores inactivos (AFK) que duran menos de 15 minutos (900 segundos), y guarda los datos limpios en formato JSONL (`matches_dataset.jsonl`).

## Requisitos Previos

Para ejecutar este proyecto, necesitarás tener instalado [Python](https://www.python.org/downloads/) (versión 3.7 o superior).

## Configuración del Entorno y Dependencias

Es muy recomendable utilizar un entorno virtual para no interferir con los paquetes globales de tu sistema. Sigue estos pasos para configurarlo:

### 1. Crear el Entorno Virtual

Abre tu terminal en la carpeta del proyecto y ejecuta el siguiente comando:

**En Windows:**
python -m venv venv
venv\Scripts\activate

**En macOS/Linux:**
python3 -m venv venv
source venv/bin/activate

### 2. Instalar las Dependencias

Una vez que el entorno virtual esté activado (verás `(venv)` en tu terminal), instala los paquetes necesarios ejecutando:

pip install -r requirements.txt

### 3. Configurar el archivo .env

El proyecto utiliza variables de entorno para mantener segura tu clave de la API de Riot. Crea un archivo llamado exactamente `.env` en la raíz de tu proyecto (al mismo nivel que tus scripts) y añade la siguiente línea, sustituyendo el valor por tu clave real:

API_KEY=RGAPI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## Uso

Una vez configurado todo, puedes ejecutar el notebook principal (`n_matches_lol_extractor.ipynb`). Gracias al uso de la base de datos SQLite (`riot_dataset.db`), el progreso se rastrea dinámicamente, lo que permite pausar y reanudar la extracción de manera segura sin perder progreso ni agotar los límites de la API en jugadores ya escaneados.

**Nota sobre los límites de la API:** Si estás utilizando una API Key temporal que caduca cada 24 horas, el script te avisará (código 401) y se detendrá cuando la clave expire. Se recomienda solicitar a Riot una API Key de producción o personal que no caduque tan rápido a través de su portal: https://developer.riotgames.com/app-type
