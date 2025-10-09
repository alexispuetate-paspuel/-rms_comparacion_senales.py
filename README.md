# -rms_comparacion_senales.py
calcular rms en las distintas graficas 
import math
import numpy as np
import matplotlib.pyplot as plt

# === FUNCIÓN RMS MUESTREADO ===
def calc_rms_movil(sig, win):
    """
    Calcula el RMS con ventana móvil sobre una señal.
    """
    rms_out = []
    for i in range(len(sig)):
        start = max(0, i - win)
        seg = sig[start:i+1]
        rms = math.sqrt(np.mean(np.square(seg)))
        rms_out.append(rms)
    return np.array(rms_out)

# === ENTRADA DE USUARIO ===
Amax = float(input("Ingresa la amplitud pico (Vp): "))
freq = float(input("Ingresa la frecuencia (Hz): "))

# === TIEMPO Y SEÑALES ===
tiempo = np.linspace(0, 2/freq, 1000)

onda_sen = Amax * np.sin(2 * math.pi * freq * tiempo)
onda_tri = Amax * (2 * np.abs(2 * (tiempo * freq - np.floor(tiempo * freq + 0.5))) - 1)
onda_cua = Amax * np.sign(np.sin(2 * math.pi * freq * tiempo))

# === RMS MÓVIL ===
win = 50
rms_sen = calc_rms_movil(onda_sen, win)
rms_tri = calc_rms_movil(onda_tri, win)
rms_cua = calc_rms_movil(onda_cua, win)

# === RMS TEÓRICO ===
RMS_sen = Amax / math.sqrt(2)
RMS_tri = Amax / math.sqrt(3)
RMS_cua = Amax

# === GRAFICACIÓN ===
plt.figure(figsize=(10, 9))

# --- Onda senoidal ---
plt.subplot(3, 1, 1)
plt.plot(tiempo, onda_sen, color='#40E0D0', label="Onda Senoidal")        # verde turquesa
plt.plot(tiempo, rms_sen, color='#DA70D6', linewidth=2, label="RMS Móvil")  # orquídea
plt.axhline(RMS_sen, color='#9370DB', linestyle='--', label=f"RMS Teórico = {RMS_sen:.3f} V")  # violeta medio
plt.title("Onda Senoidal y RMS Móvil")
plt.ylabel("Amplitud [V]")
plt.legend()
plt.grid(True)

# --- Onda triangular ---
plt.subplot(3, 1, 2)
plt.plot(tiempo, onda_tri, color='#FFB347', label="Onda Triangular")        # naranja pastel
plt.plot(tiempo, rms_tri, color='#77DD77', linewidth=2, label="RMS Móvil")  # verde menta
plt.axhline(RMS_tri, color='#FF69B4', linestyle='--', label=f"RMS Teórico = {RMS_tri:.3f} V")  # rosa fuerte
plt.title("Onda Triangular y RMS Móvil")
plt.ylabel("Amplitud [V]")
plt.legend()
plt.grid(True)

# --- Onda cuadrada ---
plt.subplot(3, 1, 3)
plt.plot(tiempo, onda_cua, color='#FFD700', label="Onda Cuadrada")          # dorado
plt.plot(tiempo, rms_cua, color='#20B2AA', linewidth=2, label="RMS Móvil")  # verde agua oscuro
plt.axhline(RMS_cua, color='#BA55D3', linestyle='--', label=f"RMS Teórico = {RMS_cua:.3f} V")  # púrpura medio
plt.title("Onda Cuadrada y RMS Móvil")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud [V]")
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()

## 📊 Ejemplo de salida

## Al ejecutar el código, el programa genera tres gráficas que muestran:
## - La señal senoidal, triangular y cuadrada.
## - El valor RMS teórico (línea punteada).
## - El valor RMS muestreado (curva en color contrastante).
