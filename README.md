# Laboratorio-3---An-lisis-espectral-de-voz
Capturar y procesar señales de voz masculinas y femeninas.

# L𝐚𝐛𝐨𝐫𝐚𝐭𝐨𝐫𝐢𝐨 𝟑 - F𝐫𝐞𝐜𝐮𝐞𝐧𝐜𝐢𝐚 𝐲 V𝐨𝐳
𝙞𝙣𝙩𝙧𝙤𝙙𝙪𝙘𝙘𝙞ó𝙣

En este laboratorio se realizó un análisis espectral de la voz  empleando técnicas de procesamiento digital de señales. Se utilizaron grabaciones de voces masculinas y femeninas para calcular parámetros como la frecuencia fundamental, frecuencia media, brillo e intensidad.

𝙤𝙗𝙟𝙚𝙩𝙞𝙫𝙤

Aplicar la Transformada de Fourier para analizar y comparar las características espectrales de voces masculinas y femeninas, identificando sus diferencias en frecuencia e intensidad para comprender mejor el comportamiento acústico de la voz humana.

𝙞𝙢𝙥𝙤𝙧𝙩𝙖𝙘𝙞ó𝙣 𝙙𝙚 𝙡𝙞𝙗𝙧𝙚𝙧𝙞𝙖𝙨
```python
import scipy.io.wavfile as wav
import matplotlib.pyplot as plt
import numpy as np
from IPython.display import Audio
from scipy.signal import find_peaks
from scipy.io import wavfile
```
Esa parte del código muestra la importación de librerías necesarias para trabajar con archivos de audio y analizarlos:`scipy.io.wavfile` y `wavfile` para leer y escribir archivos de audio `(.wav)`.`matplotlib.pyplot` para graficar señales.`numpy` para realizar operaciones numéricas y de matrices. `IPython.display.Audio` para reproducir el audio directamente en el notebook.`scipy.signal.find_peaks`para detectar picos o puntos importantes en la señal.

<h1 align="center"><i><b>𝐏𝐚𝐫𝐭𝐞 A 𝐝𝐞𝐥 𝐥𝐚𝐛𝐨𝐫𝐚𝐭𝐨𝐫𝐢𝐨</b></i></h1>
En esta parte del codigo se utiliza la función `wav.read()` de `SciPy` para cargar el archivo  y obtener su frecuencia de muestreo y datos de la señal. Si el audio tiene más de un canal, se selecciona solo uno para trabajar en mono. Luego, con `np.linspace()` de `NumPy`, se crea el eje de tiempo para cada muestra. La librería `Matplotlib (plt.plot())` se usa para graficar la señal, mostrando la amplitud frente al tiempo. Finalmente, con `Audio()` de `IPython.display`, se reproduce el sonido directamente en el entorno de ejecución.este procedimiento se realiza con cada una de las señales tanto de mujeres como para hombres.


```python
for archivo in archivos:

    print("\n======================")
    print("Analizando:", archivo)
    print("======================")

    # Leer audio
    fs, y = wavfile.read(archivo)

    # Convertir a mono si es estéreo
    if len(y.shape) > 1:
        y = y[:,0]

    y = y.astype(float)

    # -------------------------
    # 1. DOMINIO DEL TIEMPO
    # -------------------------

    t = np.arange(len(y)) / fs

    plt.figure(figsize=(8,4))
    plt.plot(t, y, color="#FF00FF")
    plt.title("Señal - " + archivo)
    plt.xlabel("Tiempo (s)")
    plt.ylabel("Amplitud")
    plt.grid(True)
    plt.show()

    # -------------------------
    # 2. TRANSFORMADA DE FOURIER
    # -------------------------

    N = len(y)
    Y = np.fft.fft(y)
    f = np.fft.fftfreq(N, 1/fs)
    magnitud = np.abs(Y)

    # Solo frecuencias positivas
    f_pos = f[:N//2]
    mag_pos = magnitud[:N//2]

    # -------------------------
    # 3. FILTRAR 50–1500 Hz
    # -------------------------

    mask = (f_pos >= 50) & (f_pos <= 1500)

    f_range = f_pos[mask]
    mag_range = mag_pos[mask]

    # -------------------------
    # 4. ESPECTRO SEMILOG EN X
    # -------------------------

    plt.figure(figsize=(8,4))
    plt.semilogx(f_range, mag_range, color="#FF00FF")
    plt.title("Espectro de frecuencias (50–1500 Hz) - " + archivo)
    plt.xlabel("Frecuencia (Hz) - Escala log")
    plt.ylabel("Magnitud")
    plt.xlim(50,1500)
    plt.grid(True, which="both")
    plt.show()

    # -------------------------
    # 5. FRECUENCIA FUNDAMENTAL
    # -------------------------

    indice = np.argmax(mag_range)
    f0 = f_range[indice]

    # -------------------------
    # 6. FRECUENCIA MEDIA
    # -------------------------

    f_media = np.sum(f_range * mag_range) / np.sum(mag_range)

    # -------------------------
    # 7. BRILLO
    # -------------------------

    brillo = f_media

    # -------------------------
    # 8. ENERGÍA
    # -------------------------

    energia = np.sum(y**2)

    # -------------------------
    # RESULTADOS
    # -------------------------

    print("Frecuencia fundamental:", round(f0,2), "Hz")
    print("Frecuencia media:", round(f_media,2), "Hz")
    print("Brillo:", round(brillo,2))
    print("Energia:", energia)
```
## resultados

