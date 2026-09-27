# Generador de PDFs de Pagos con Google Colab

Este proyecto permite **generar automáticamente comprobantes de pago en formato PDF** a partir de los datos almacenados en una hoja de cálculo de Google Sheets.

El programa utiliza **Google Colab, Google Drive, Google Sheets, Pandas y Pillow**. Por cada registro encontrado en la hoja de cálculo, se genera un PDF utilizando una plantilla gráfica y se colocan los datos correspondientes en posiciones predeterminadas.

## ¿Cómo funciona?

El flujo del programa es el siguiente:

1. Se conecta Google Drive con Google Colab.
2. Se establece la ruta de la carpeta donde se encuentra el proyecto.
3. Se conecta con Google Sheets mediante la autenticación de la cuenta de Google.
4. Lee los registros de la hoja de cálculo.
5. Utiliza `Recibo-Plantilla.png` como plantilla del comprobante.
6. Inserta los datos de cada registro en las coordenadas configuradas.
7. Genera un archivo PDF por cada registro.
8. Guarda automáticamente los PDFs dentro de la carpeta `pago-doc`.

## Estructura del proyecto

La carpeta del proyecto en Google Drive debe contener:

```text
Generador-Pagos/
├── Generar_PDF_Pagos.ipynb
├── Recibo-Plantilla.png
└── font.ttf
```

La carpeta `pago-doc` **no es necesario crearla manualmente**, ya que el programa la genera automáticamente:

```text
Generador-Pagos/
├── Generar_PDF_Pagos.ipynb
├── Recibo-Plantilla.png
├── font.ttf
└── pago-doc/
    ├── Nombre1.pdf
    ├── Nombre2.pdf
    └── ...
```

## Requisitos

* Cuenta de Google.
* Google Colab.
* Google Drive.
* Google Sheets.
* Python.
* Una plantilla en formato PNG.
* Una fuente en formato `.ttf`.

Las librerías utilizadas son:

* `pandas`
* `gspread`
* `Pillow`
* `google-auth`

## Configuración

### 1. Crear la carpeta del proyecto

Crea una carpeta en Google Drive para almacenar los archivos del proyecto.

Por ejemplo:

```text
Python-Cloud/
└── Generador-Pagos/
```

Dentro de esta carpeta coloca:

```text
Generar_PDF_Pagos.ipynb
Recibo-Plantilla.png
font.ttf
```

### 2. Configurar la ruta

En el notebook encontrarás:

```python
os.chdir('ruta de la carpeta del proyecto')
```

Reemplaza `'ruta de la carpeta del proyecto'` por la ruta correspondiente a tu carpeta.

La ruta dependerá de dónde hayas colocado la carpeta en tu Google Drive.

### 3. Conectar Google Drive

El código utiliza:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Al ejecutar esta sección, Google Colab solicitará autorización para acceder a tu Google Drive.

### 4. Configurar Google Sheets

El programa busca una hoja de cálculo llamada:

```text
Presupuesto
```

Dentro de ella debe existir una hoja llamada:

```text
pago
```

La hoja debe contener las siguientes columnas:

```text
Fecha
ID
Nombre
Teléfono
Dirección
Horas Trabajadas
Pago por Hora
Pago Total
```

Ejemplo:

| Fecha      | ID  | Nombre     | Teléfono  | Dirección | Horas Trabajadas | Pago por Hora | Pago Total |
| ---------- | --- | ---------- | --------- | --------- | ---------------: | ------------: | ---------: |
| 01/09/2026 | 001 | Juan Pérez | 999999999 | Lima      |                8 |            20 |        160 |
| 02/09/2026 | 002 | Ana López  | 988888888 | Lima      |                6 |            25 |        150 |

Los datos utilizados en el proyecto pueden ser reemplazados por los datos propios del usuario.

## Generación de los PDFs

El programa utiliza un diccionario de coordenadas para determinar dónde colocar cada dato dentro de la plantilla:

```python
coordenadas = {
    'Fecha': (1020,311),
    'ID': (1280,311),
    'Nombre': (147,530),
    'Teléfono': (495,530),
    'Dirección': (811,530),
    'Horas Trabajadas': (1230,800),
    'Pago por Hora': (1180,885),
    'Pago Total': (1180,1025)
}
```

Estas coordenadas pueden modificarse dependiendo del diseño de la plantilla.

La fuente utilizada también puede modificarse:

```python
fuente = ImageFont.truetype('font.ttf', 25)
```

El segundo parámetro corresponde al tamaño de la fuente.

## Carpeta de salida

Los PDFs se guardan automáticamente en:

```text
pago-doc/
```

El programa crea esta carpeta mediante:

```python
os.makedirs(carpeta_doc, exist_ok=True)
```

Por lo tanto, **no es necesario crearla previamente**.

El nombre de cada PDF se genera utilizando el nombre registrado en la columna `Nombre`.

Por ejemplo:

```text
Juan Pérez.pdf
Ana López.pdf
Carlos García.pdf
```

## Autenticación

Para acceder a Google Sheets, el notebook utiliza la autenticación de Google:

```python
auth.authenticate_user()
creds, _ = google.auth.default()
gc = gspread.authorize(creds)
```

Cada usuario debe autorizar su propia cuenta de Google cuando ejecute el notebook.

No es necesario colocar contraseñas, tokens ni credenciales dentro del código.

## Uso del proyecto

1. Descarga o clona este repositorio.
2. Abre `Generar_PDF_Pagos.ipynb` en Google Colab.
3. Coloca el notebook, `Recibo-Plantilla.png` y `font.ttf` dentro de una misma carpeta de Google Drive.
4. Ejecuta:

```python
drive.mount('/content/drive')
```

5. Configura la ruta de la carpeta en:

```python
os.chdir('ruta de la carpeta del proyecto')
```

6. Crea o utiliza una hoja de cálculo llamada `Presupuesto`.
7. Dentro de ella, crea una hoja llamada `pago`.
8. Asegúrate de que las columnas tengan los nombres requeridos.
9. Ejecuta las celdas del notebook.
10. Autoriza el acceso a tu cuenta de Google cuando Colab lo solicite.
11. Los comprobantes generados aparecerán automáticamente dentro de `pago-doc`.

## Archivos que no deben subirse al repositorio

No es necesario subir:

* La hoja de cálculo `Presupuesto`.
* Los PDFs generados.
* La carpeta `pago-doc`.
* Archivos con credenciales o tokens.
* Otros notebooks que no pertenezcan al proyecto.

Los datos utilizados en el proyecto deben ser datos de prueba o datos para los cuales se tenga autorización de uso.

## Personalización

Puedes adaptar el proyecto modificando:

* La plantilla `Recibo-Plantilla.png`.
* La fuente `font.ttf`.
* El tamaño de la fuente.
* El color del texto.
* Las coordenadas de cada campo.
* Las columnas utilizadas en Google Sheets.
* El nombre de la hoja de cálculo.
* El nombre de la pestaña de Google Sheets.

## Tecnologías utilizadas

* **Python**
* **Google Colab**
* **Google Drive**
* **Google Sheets**
* **Pandas**
* **gspread**
* **Pillow**

## Licencia

Este proyecto puede utilizarse como base para automatizar la generación de documentos PDF a partir de datos almacenados en Google Sheets.

