# League of Legends Match Scraper (Riot API)

Este proyecto contiene una serie de scripts en Python diseñados para extraer datos de partidas clasificatorias (Solo/DuoQ) de League of Legends utilizando la API oficial de Riot Games.

El script está configurado para obtener datos masivos de un parche específico (por defecto, el 16.18), realizando los siguientes pasos:

1. Obtención de PUUIDs de jugadores divididos por Elo (desde Hierro hasta Challenger).
2. Recopilación de los IDs de las partidas jugadas en el rango de fechas del parche.
3. Descarga de la información detallada de cada partida, guardándola en formato JSONL y manejando automáticamente los límites de peticiones (Rate Limits) de la API.

## Requisitos Previos

Para ejecutar este proyecto, necesitarás tener instalado [Python](https://www.python.org/downloads/) (versión 3.7 o superior).

## Configuración del Entorno y Dependencias

Es muy recomendable utilizar un entorno virtual para no interferir con los paquetes globales de tu sistema. Sigue estos pasos para configurarlo:

### 1. Crear el Entorno Virtual

Abre tu terminal en la carpeta del proyecto y ejecuta el siguiente comando:

**En Windows:**

```bash
python -m venv venv
venv\Scripts\activate
```

**En macOS/Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 2. Instalar las Dependencias

Una vez que el entorno virtual esté activado (verás `(venv)` en tu terminal), instala los paquetes necesarios ejecutando:

```bash
pip install -r requirements.txt
```

### 3. Configurar el archivo `.env`

El proyecto utiliza variables de entorno para mantener segura tu clave de la API de Riot.

Crea un archivo llamado exactamente `.env` en la raíz de tu proyecto (al mismo nivel que tus scripts) y añade la siguiente línea, sustituyendo el valor por tu clave real:

```env
API_KEY=RGAPI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

## Uso

Una vez configurado todo, puedes ejecutar los scripts. Ten en cuenta que los archivos de salida se irán generando de forma incremental (`puuids_per_elo.jsonl`, `match_to_process.txt`, y `matches_raw_dataset.jsonl`), lo que permite pausar y reanudar la extracción si se alcanza el límite de la API o cierras el programa.
Por otro lado, si tienes la API key que caduca cada 24 horas, se va a parar al buscar las partidas, por lo que hay que hacerlo de 50k en 50k jugadores para no tardar más de 24 horas o pedirle a Riot una API key que no caduque tan pronto desde este sitio: https://developer.riotgames.com/app-type
