# 🚗 Detección Automática de Matrículas con OpenCV y Tesseract

Proyecto de visión por computador desarrollado en Python para la detección automática de matrículas de vehículos mediante técnicas de procesamiento de imagen y reconocimiento óptico de caracteres (OCR).

## 📌 Descripción

Este proyecto implementa un pipeline completo de procesamiento de imagen capaz de:

- Leer y preprocesar imágenes de vehículos.
- Detectar bordes mediante el algoritmo de Canny.
- Aplicar operaciones morfológicas para limpiar y mejorar la segmentación.
- Detectar automáticamente la región correspondiente a la matrícula.
- Corregir la orientación de la matrícula.
- Extraer el texto de la matrícula utilizando OCR con Tesseract.

El desarrollo ha sido realizado en un notebook de Jupyter (`.ipynb`) utilizando bibliotecas de visión artificial y procesamiento científico en Python.

---

# 🧠 Tecnologías utilizadas

## Lenguaje

- Python 3.11

## Librerías principales

- OpenCV (`cv2`)
- NumPy
- Matplotlib
- SciPy
- scikit-image
- pytesseract

---

# 📂 Estructura del proyecto

```bash
.
├── images/
│   ├── auto_1.jpg
│   ├── auto_2.jpg
│   ├── auto_3.jpg
│   ├── auto_4.jpg
│   └── auto_5.jpg
│
├── notebook.ipynb
└── README.md
```

---

# ⚙️ Funcionamiento del pipeline

## 1️⃣ Lectura y redimensionado de la imagen

La imagen del vehículo se carga mediante OpenCV y posteriormente se redimensiona para trabajar con un tamaño homogéneo.

```python
imagen_bgr = cv2.imread(ruta_imagen)
imagen_rgb = cv2.cvtColor(imagen_bgr, cv2.COLOR_BGR2RGB)
imagen_redimensionada = cv2.resize(imagen_rgb, (768, 512))
```

---

## 2️⃣ Detección de bordes con Canny

Se aplica un filtrado Gaussiano para reducir ruido y posteriormente se utiliza el detector de bordes de Canny.

### Procesos aplicados

- Conversión a escala de grises.
- Filtro Gaussiano 3x3.
- Detección de bordes.
- Dilatación para conectar regiones.

```python
bordes = cv2.Canny(imagen_gris, umbral_min, umbral_max)
imagen_dilatada = cv2.dilate(bordes, kernel_2x2, iterations=1)
```

---

## 3️⃣ Operaciones morfológicas

Se realizan distintas operaciones morfológicas para mejorar la máscara binaria:

- Relleno de huecos.
- Apertura vertical.
- Apertura horizontal.
- Eliminación de pequeños artefactos.

```python
bordes_rellenos = binary_fill_holes(imagen_dilatada > 0).astype(np.uint8)
mascara_limpia = remove_small_objects(mascara_booleano, min_size=100, connectivity=8)
```

---

## 4️⃣ Detección automática de la matrícula

Se etiquetan las regiones detectadas y se analiza su geometría.

La matrícula se selecciona utilizando la proporción entre:

- `minor_axis_length`
- `major_axis_length`

El objeto con menor proporción es considerado la matrícula.

```python
proporciones.append(minor_axis / major_axis)
indice_menor_proporcion = np.argmin(proporciones)
```

Posteriormente:

- Se obtienen los contornos.
- Se dibuja la región detectada.
- Se calcula la bounding box.

---

## 5️⃣ Corrección de orientación

El sistema calcula la orientación de la matrícula detectada y aplica una rotación automática para alinearla horizontalmente.

```python
rotation_matrix = cv2.getRotationMatrix2D(center, angle, 1.0)
rotated_crop = cv2.warpAffine(crop_imagen, rotation_matrix, ...)
```

Esto mejora considerablemente el reconocimiento OCR.

---

## 6️⃣ Reconocimiento óptico de caracteres (OCR)

Se utiliza `pytesseract` para convertir el contenido de la matrícula en texto.

```python
string = pytesseract.image_to_string(rotated_contrast, config='--psm 10')
string = ''.join(filter(str.isalnum, string))
```

Finalmente, el texto detectado se muestra sobre la imagen.

---

# ▶️ Instalación

## 1. Clonar el repositorio

```bash
git clone https://github.com/usuario/as_deteccion_matriculas.git
cd as_deteccion_matriculas
```

---

## 2. Instalar dependencias

```bash
pip install opencv-python numpy matplotlib scipy scikit-image pytesseract
```

---

## 3. Instalar Tesseract OCR

### Windows

Descargar desde:

https://github.com/UB-Mannheim/tesseract/wiki

Añadir la ruta del ejecutable en el notebook:

```python
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract'
```



# 📊 Resultados

El sistema es capaz de:

✅ Detectar automáticamente la matrícula.

✅ Corregir su orientación.

✅ Extraer el texto contenido en la imagen.

✅ Mostrar visualmente cada etapa del procesamiento.

---

# 🧪 Posibles mejoras futuras

- Mejorar la robustez ante iluminación variable.
- Implementar detección mediante redes neuronales.
- Añadir soporte para vídeo en tiempo real.
- Utilizar modelos OCR más avanzados.
- Detectar múltiples matrículas simultáneamente.

---

# 📚 Conceptos trabajados

- Procesamiento digital de imágenes.
- Visión por computador.
- Segmentación.
- Operaciones morfológicas.
- Detección de contornos.
- OCR (Optical Character Recognition).
- Transformaciones geométricas.

---



