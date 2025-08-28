# Oscilloscope

## 1. Eye-Diagram
```
import sys
import numpy as np
import matplotlib
matplotlib.interactive(False)
import matplotlib.pyplot as plt
import csv 

if len(sys.argv) < 3:
  exit(1)

name = sys.argv[1].replace(".csv", "")
print(f"name: {name}")

bit_rate = np.float64(sys.argv[2]) * 1.0e6
print(f"bit_rate: {bit_rate} MHz")
samples_total = 6.25e9
samples_per_window = int(samples_total / bit_rate) * 2
print(f"samples_per_window: {samples_per_window}")

with open(f"{name}.csv") as f:
  reader = csv.reader(f)
  signal = [row for row in reader]

signal = np.array(signal[11:], dtype=np.float64)
n_windows = int(len(signal) / samples_per_window)
n_samples = n_windows * samples_per_window

x = np.arange(samples_per_window)
for a in range(1, len(signal[0])):
  reshaped = signal[:n_samples, a].reshape((n_windows, samples_per_window))
  print(f"reshaped: {reshaped.shape}")
  plt.figure()
  for y in reshaped:
    plt.plot(x, y, color="blue", alpha=1.0)
  plt.gca().yaxis.set_major_formatter(plt.FormatStrFormatter('%.1f'))
  plt.ylim(-1.0, 4.0)
  plt.yticks([0.0, 3.3])
  plt.title("Eye Diagram")
  plt.xlabel("Samples")
  plt.ylabel("Voltage [V]")
  plt.savefig(f"{name}_ch{a}.jpg")

plt.figure()
colors = [None, 'orange', 'blue', 'green']
for a in range(1, len(signal[0])):
  reshaped = signal[:n_samples, a].reshape((n_windows, samples_per_window))
  print(f"reshaped: {reshaped.shape}")
  for y in reshaped:
    plt.plot(x, y, color=colors[a], alpha=0.2)

plt.gca().yaxis.set_major_formatter(plt.FormatStrFormatter('%.1f'))
plt.ylim(-1.0, 4.0)
plt.yticks([0.0, 3.3])
plt.title("Eye Diagram")
plt.xlabel("Samples")
plt.ylabel("Voltage [V]")
plt.savefig(f"{name}.jpg")
```
