# CNN para clasificación de eventos Cherenkov — Super-Kamiokande

Taller de introducción a Redes Neuronales Convolucionales (CNN) usando imágenes reales del detector **Super-Kamiokande** (Japón). Diseñado para estudiantes de preparatoria interesados en sistemas computacionales e inteligencia artificial.

---

## ¿Qué hace este proyecto?

Entrena una CNN que distingue dos tipos de eventos de partículas a partir de la morfología de su **anillo Cherenkov**:

| Clase | Partícula | Aspecto del anillo |
|-------|-----------|-------------------|
| `mu_like` | Muón (µ) | Borde **nítido y continuo** |
| `e_like` | Electrón / positrón | Borde **difuso o granulado** |

Esta clasificación es fundamental para los análisis de oscilación de neutrinos que realiza la colaboración Super-Kamiokande.

> 🔗 Monitor en tiempo real del detector:  
> [https://www-sk.icrr.u-tokyo.ac.jp/realtimemonitor/](https://www-sk.icrr.u-tokyo.ac.jp/realtimemonitor/)

---

## Contenido del repositorio

```
cnn_neutrinos_sk/
├── Taller_SK_Cherenkov_CNN_v07.ipynb   # Notebook de entrenamiento — construye y entrena la CNN
├── Taller_SK_Cherenkov_CNN_v072.ipynb  # Notebook de inferencia — carga el modelo ya entrenado
├── SK_mu_e_dataset_v01.zip             # Dataset de imágenes del barrel (train/val/test)
└── README.md
```

---

## Estructura del dataset

```
SK_mu_e_dataset_v01/
  train/          ← 70% — la red aprende aquí
    mu_like/
    e_like/
  val/            ← 15% — ajuste de hiperparámetros
    mu_like/
    e_like/
  test/           ← 15% — evaluación final
    mu_like/
    e_like/
```

---

## Cómo usar los notebooks

### Opción A — Google Colab (recomendado)

1. Notebook de entrenamiento (construye la CNN desde cero):

   [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/echucuan/cnn_neutrinos_sk/blob/main/Taller_SK_Cherenkov_CNN_v07.ipynb)

2. Notebook de inferencia (carga el modelo ya entrenado y clasifica imágenes nuevas):

   [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/echucuan/cnn_neutrinos_sk/blob/main/Taller_SK_Cherenkov_CNN_v072.ipynb)

3. En Colab: `Entorno de ejecución` → `Cambiar tipo de entorno de ejecución` → **GPU (T4)**

4. Sube `SK_mu_e_dataset_v01.zip` a `Mi unidad/colab_shared/` en Google Drive

5. Ejecuta las celdas en orden con `Shift + Enter`

### Opción B — Local

```bash
git clone https://github.com/echucuan/cnn_neutrinos_sk.git
cd cnn_neutrinos_sk
pip install tensorflow numpy matplotlib scikit-learn pillow

# Entrenamiento
jupyter notebook Taller_SK_Cherenkov_CNN_v07.ipynb

# Inferencia (después de entrenar y guardar el modelo)
jupyter notebook Taller_SK_Cherenkov_CNN_v072.ipynb
```

---

## Arquitectura de la CNN

```
Entrada: 224 × 224 × 1  (imagen en escala de grises)
        ↓
  Conv2D(32)  + ReLU → MaxPool → 112 × 112 × 32
        ↓
  Conv2D(64)  + ReLU → MaxPool →  56 ×  56 × 64
        ↓
  Conv2D(128) + ReLU → MaxPool →  28 ×  28 × 128
        ↓
  Flatten → Dense(128) + ReLU → Dropout(0.5)
        ↓
  Dense(1) + Sigmoid → p(mu_like) ∈ [0, 1]
```

**Total de parámetros:** ~12.9 M  
**Optimizador:** Adam (lr = 1e-3)  
**Función de pérdida:** Binary Crossentropy  
**Épocas:** 20  

---

## Requisitos

```
tensorflow >= 2.15
numpy
matplotlib
scikit-learn
pillow
```

---

## Contexto científico

Las imágenes provienen del **monitor público en tiempo real** de Super-Kamiokande. El detector registra eventos continuamente; este taller usa capturas del panel central (barrel) donde los anillos Cherenkov son más visibles.

La clasificación mu/e es un paso clave en los análisis de:
- Oscilación de neutrinos atmosféricos
- Búsqueda de decaimiento del protón
- Detección de neutrinos de supernovas

---

## Licencia

MIT — libre para uso educativo y de investigación.
