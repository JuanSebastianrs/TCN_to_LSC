```markdown
# Traductor **LSC → Voz** en Tiempo Real  🎙️🤟

Sistema completo que **detecta, reconoce y vocaliza**  
diez gestos básicos de la **Lengua de Señas Colombiana (LSC)**  
empleando únicamente una cámara web y la **CPU**.

| Conjunto | Accuracy | Macro-F<sub>1</sub> |
|----------|----------|---------------------|
| **Test** | 0.981    | 0.981              |

<p align="center">
  <img src="Figures/demo_gui.gif" width="600">
</p>

---

## 🗂️ Estructura del repositorio

```

.
├─ capture\_samples.py      # Paso 1 – Grabación automática

├─ normalize\_samples.py    # Paso 2 – Interpolación a 15 frames

├─ create\_keypoints.py     # Paso 3 – Extracción de 1 662 key-points

├─ prepare\_dataset.py      # Paso 4 – Split estratificado 70/15/15

├─ model.py                # Paso 5 – Red TCN + Attention

├─ training\_model.py       # Paso 6 – Entrenamiento

├─ confusion\_matrix.py     # Paso 7 – Métricas y gráficas

├─ plot\_pr\_curves.py       # Extensión: curvas PR

├─ latent\_tsne\_umap.py     # Extensión: t-SNE / UMAP

├─ main.py                 # Paso 8 – GUI PyQt5 en tiempo real

├─ text\_to\_speech.py       # Paso 9 – Síntesis de voz

├─ data/                   # Key-points y splits serializados

├─ frame\_actions/          # Frames JPG por gesto

└─ models/                 # Modelo *.keras* y words.json

````

---

## ⚡ Instalación rápida

```bash
# 1 Crear y activar entorno virtual
python -m venv lsc_env
# Windows ➜ lsc_env\Scripts\activate
source lsc_env/bin/activate

# 2 Instalar dependencias (CPU-only)
pip install -r requirements.txt
#   – TensorFlow 2.16.1
#   – mediapipe
#   – opencv-python
#   – PyQt5
#   – gTTS, pygame
#   – seaborn, umap-learn …

# 3 Descargar modelo pre-entrenado
curl -L -o models/actions_15.keras https://…/actions_15.keras
````

> El sistema corre a \~25 fps en un portátil i5; **no se requiere GPU**.

---

## ▶️ Demo en vivo

```bash
python main.py
```

1. Se abre la webcam.
2. Realice un gesto completo; la aplicación lo detecta automáticamente.
3. Al acabar, se muestra el texto y se reproduce la voz en castellano.

---

## 🔬 Entrenamiento desde cero

```bash
# 1 Capturar datos (≈200 muestras por gesto)
python capture_samples.py --word hola

# 2 Normalizar e indexar key-points
python normalize_samples.py
python create_keypoints.py

# 3 Generar splits y entrenar
python prepare_dataset.py
python training_model.py            # produce models/actions_15.keras
```

---

## 📈 Evaluación y gráficos adicionales

```bash
python confusion_matrix.py          # matriz + reporte
python plot_pr_curves.py            # curvas Precision–Recall
python latent_tsne_umap.py          # mapa t-SNE / UMAP
```

---

## ✨ Componentes clave

| Módulo                   | Tecnología          | Rol principal                                 |
| ------------------------ | ------------------- | --------------------------------------------- |
| Detección / Tracking     | MediaPipe           | 1 662 landmarks (pose + face + hands)         |
| Normalización temporal   | OpenCV              | Interpolación / muestreo a 15 frames          |
| Clasificación secuencial | **TCN + Attention** | RF 31 frames, 3.5 M parámetros, 98 % accuracy |
| Interfaz gráfica         | PyQt5               | Webcam, overlay de key-points, texto dinámico |
| Síntesis de voz          | gTTS + pygame       | Locución en español con baja latencia         |

---

## 📝 Descripción breve

El proyecto integra **detección corporal, seguimiento temporal,
normalización, red convolucional dilatada con atención escalar y
síntesis de voz** para ofrecer un traductor LSC-a-Audio portátil,
de código abierto y totalmente funcional en CPU.

---

## 📚 Créditos

**Autores:**  Juan Sebastian Rodriguez Salazar y Johan Santiago Caro Valencia / Grupo – Visión Computacional, 2025-I

Agradecimientos al repositorio base [@ronvidev](https://github.com/ronvidev/modelo_lstm_lsp)
por la lógica original de captura.

```
```
