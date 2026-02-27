# 🗺️ Flask + OpenStreetMap — Buscador de Lugares

Aplicación web educativa desarrollada con **Flask** y la API de **OpenStreetMap (Nominatim)** que permite buscar cualquier lugar del mundo y visualizarlo en un mapa interactivo.

---

## Descripción

Este proyecto demuestra el consumo de una API REST externa desde una aplicación web Python/Flask. El usuario ingresa el nombre de un lugar y la aplicación consulta la API de Nominatim para obtener sus coordenadas, mostrando el resultado en un mapa interactivo renderizado con **Leaflet.js**.

---

##  Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| Python 3 | Lenguaje principal |
| Flask | Framework web |
| Requests | Peticiones HTTP a la API |
| Nominatim (OpenStreetMap) | API de geolocalización |
| Leaflet.js | Renderizado del mapa interactivo |
| HTML / CSS | Interfaz de usuario |
| Jinja2 | Motor de plantillas |

---

##  Estructura del proyecto

```
flask_hola_mundo/
│
├── app/
│   ├── app.py               # Lógica principal de la aplicación
│   └── templates/
│       ├── index.html       # Página de inicio
│       └── map.html         # Página del mapa
│
├── static/
│   └── style.css            # Estilos personalizados
│
└── venv/                    # Entorno virtual 
```

> <img width="351" height="261" alt="image" src="https://github.com/user-attachments/assets/31fcc3f3-87d3-4e0b-9934-798fbcb17646" />

---

##  Cómo ejecutar la aplicación

Con el entorno virtual activado, desde la carpeta del proyecto:

```bash
python app.py
```

Luego abre tu navegador en:

```
http://127.0.0.1:5000
```

---

##  API utilizada — Nominatim (OpenStreetMap)

| Campo | Detalle |
|---|---|
| Tipo | API REST |
| Método | GET |
| Endpoint | `https://nominatim.openstreetmap.org/search` |
| Autenticación | No requiere API Key |
| Formato de respuesta | JSON |

### Parámetros utilizados

| Parámetro | Descripción |
|---|---|
| `q` | Texto del lugar a buscar |
| `format` | Formato de respuesta (`json`) |
| `limit` | Número máximo de resultados |

>  Es obligatorio enviar un header `User-Agent` en cada petición según las políticas de uso de Nominatim.

---

##  Explicación del código principal

### `app.py`

```python
from flask import Flask, render_template, request
import requests

app = Flask(__name__)
```

- **`Flask`** — Clase principal para crear la instancia de la aplicación web.
- **`render_template`** — Genera páginas HTML dinámicas usando el motor Jinja2.
- **`request`** — Permite acceder a datos de formularios y peticiones HTTP entrantes.
- **`import requests`** — Librería para realizar peticiones HTTP *salientes* hacia APIs externas (diferente al `request` de Flask).
- **`Flask(__name__)`** — Crea la app usando el nombre del módulo actual para localizar plantillas y archivos estáticos.

### Ruta principal `/`

```python
@app.route('/')
def index():
    return render_template('index.html')
```

Muestra la página de inicio con el enlace para buscar un lugar.

### Ruta `/buscar`

```python
@app.route('/buscar', methods=['GET', 'POST'])
def buscar():
    if request.method == 'POST':
        lugar = request.form['lugar']
        url = "https://nominatim.openstreetmap.org/search"
        params = {"q": lugar, "format": "json", "limit": 1}
        headers = {"User-Agent": "Flask-Educational-App"}
        response = requests.get(url, params=params, headers=headers)
        data = response.json()
        if data:
            lat = data[0]['lat']
            lon = data[0]['lon']
            nombre = data[0]['display_name']
            return render_template('map.html', lat=lat, lon=lon, nombre=nombre)
    return render_template('map.html', error=True)
```

> <img width="480" height="603" alt="image" src="https://github.com/user-attachments/assets/2fc908c1-a3fe-49fa-a82b-0c00a631ae41" />

Recibe el nombre del lugar, consulta la API de Nominatim y pasa las coordenadas al template `map.html`.

---

##  Capturas de pantalla

### Página de inicio

> <img width="1364" height="671" alt="image" src="https://github.com/user-attachments/assets/227d6be1-13f7-4d94-a304-4a4821b3dc2d" />


---

### Búsqueda de un lugar

> <img width="1365" height="674" alt="image" src="https://github.com/user-attachments/assets/b5c0caa7-4a92-470c-917d-b530362b2029" />


---

### Resultado en el mapa

> <img width="1365" height="674" alt="image" src="https://github.com/user-attachments/assets/3362287b-24f4-405f-b53e-03bdcc51af93" />


---

##  Autor

**Vanesa Monserrat Medrano Hernández**  
> Materia: Desarrollo Web Profesional Unidad II
> Institución: Universidad Tecnológica del Norte de Guanajuato
