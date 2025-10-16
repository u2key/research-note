# Eye-Diagram Generation

## 1. Common Part
```python
import sys
import numpy as np
import matplotlib
matplotlib.interactive(False)
import matplotlib.pyplot as plt
import csv

alpha = 1.0
if len(sys.argv) < 3:
  exit(1)
name = sys.argv[1].replace(".csv", "")
print(f"name: {name}")
bit_rate = np.float64(sys.argv[2]) * 1.0e6
print(f"bit_rate = {bit_rate} [MHz]")
window_interval = 1.0 / bit_rate
print(f"window_interval = {window_interval} [sec.]")

with open(f"{name}.csv") as f:
  reader = csv.reader(f)
  signal = [row for row in reader]
if signal[4][0] != "Sample Interval":
  print(f"Sample Interval is not found")
  exit(1)
sample_interval = np.float64(signal[4][1])
samples_per_window = window_interval / sample_interval
samples_per_window_int = int(samples_per_window)
print(f"samples_per_window = {samples_per_window} [samples]")
print(f"samples_per_window_int = {samples_per_window_int} [samples]")
bit_middle = samples_per_window / 2
print(f"bit_middle = {bit_middle} [samples]")

signal_start = 11
signal_end = samples_per_window * 11 + 10
signal = np.array(signal[signal_start:], dtype=np.float64)
# signal = np.array(signal[signal_start:signal_end], dtype=np.float64)
print(f"signal = {signal}")
```

## 2. Generate Each Channel Eye-Diagram
```python
x = np.arange(samples_per_window_int)
for a in range(1, len(signal[0])):
  plt.figure()
  axes = plt.axes()
  axes.set_facecolor('black')
  window_num = 0
  while True:
    samples_in_window_index_start = int(window_num * samples_per_window)
    samples_in_window_index_end = samples_in_window_index_start + samples_per_window_int
    window_num = window_num + 1
    if samples_in_window_index_end >= len(signal):
      break
    samples_in_window = signal[samples_in_window_index_start:samples_in_window_index_end, a].flatten()
    plt.plot(x, samples_in_window, color="blue", alpha=alpha)
  plt.plot([bit_middle, bit_middle], [-1.0, 4.0], color="red")
  plt.gca().yaxis.set_major_formatter(plt.FormatStrFormatter('%.1f'))
  plt.ylim(-1.0, 4.0)
  plt.yticks([0.0, 3.3])
  plt.title(f"Eye Diagram of {bit_rate:.1e} bps")
  plt.xlabel("Samples")
  plt.ylabel("Voltage [V]")
  plt.savefig(f"alpha{alpha}_{name}_ch{a}.jpg")
```

## 3. Generate All Channel Eye-Diagram
```python
plt.figure()
axes = plt.axes()
axes.set_facecolor('black')
colors = [None, 'orange', 'blue', 'green', 'white']
for a in range(1, len(signal[0])):
  window_num = 0
  while True:
    samples_in_window_index_start = int(window_num * samples_per_window)
    samples_in_window_index_end = samples_in_window_index_start + samples_per_window_int
    window_num = window_num + 1
    if samples_in_window_index_end >= len(signal):
      break
    samples_in_window = signal[samples_in_window_index_start:samples_in_window_index_end, a].flatten()
    plt.plot(x, samples_in_window, color=colors[a], alpha=alpha)
plt.plot([bit_middle, bit_middle], [-1.0, 4.0], color="red")
plt.gca().yaxis.set_major_formatter(plt.FormatStrFormatter('%.1f'))
plt.ylim(-1.0, 4.0)
plt.yticks([0.0, 3.3])
plt.title(f"Eye Diagram of {bit_rate:.1e} bps")
plt.xlabel("Samples")
plt.ylabel("Voltage [V]")
plt.savefig(f"alpha{alpha}_{name}.jpg")
```

## 4. Sample Continuous Bits
```python
def transmit_bit(a, bits_per_data=12):
  bit_num = a % bits_per_data
  if bit_num == 0:
    return 0
  elif bit_num >= 1 and bit_num <= 8:
    data = list(f"{int(a / bits_per_data + 1) % 256:0>8b}")
    return int(data[8-bit_num])
  elif bit_num == 9:
    data = list(f"{int(a / bits_per_data + 1) % 256:0>8b}")
    parity = 0
    for b in range(8):
      parity += int(data[7-b])
    return parity % 2
  elif bit_num >= 10:
    return 1

continuous = [[[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))],
              [[] for _ in range(1, len(signal[0]))]]
samples_per_1window_int = int(samples_per_window * 2.0)
samples_per_2window_int = int(samples_per_window * 3.0)
samples_per_3window_int = int(samples_per_window * 4.0)
samples_per_4window_int = int(samples_per_window * 5.0)
samples_per_5window_int = int(samples_per_window * 6.0)
samples_per_6window_int = int(samples_per_window * 7.0)
samples_per_7window_int = int(samples_per_window * 8.0)
samples_per_8window_int = int(samples_per_window * 9.0)
samples_per_9window_int = int(samples_per_window * 10.0)
bit_pool = [transmit_bit(a) for a in range(15)]
bit_num = 0
samples_in_window_index_start = [0, 0, 0, 0]
break_all_loop = False
while not break_all_loop:
  same_num = 0
  for a in range(len(bit_pool)):
    if bit_pool[a] == bit_pool[0]:
      same_num += 1
    else:
      break
  if same_num == 1:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_1window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[0][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 2:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_2window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[1][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 3:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_3window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[2][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 4:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_4window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[3][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 5:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_5window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[4][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 6:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_6window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[4][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 6:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_6window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[5][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 7:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_7window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[6][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 8:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_8window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[7][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  elif same_num == 9:
    for a in range(1, len(signal[0])):
      samples_in_window_index_end = samples_in_window_index_start[a] + samples_per_9window_int
      if samples_in_window_index_end >= len(signal):
        break
      continuous[8][a-1].append(signal[samples_in_window_index_start[a]:samples_in_window_index_end, a].flatten())
  bit_num_last = bit_num
  samples_in_window_index_start_last = deepcopy(samples_in_window_index_start)
  for a in range(1, len(signal[0])):
    samples_in_window_index_start[a] = samples_in_window_index_start[a] + int(samples_per_window / 10)
    while True:
      samples_in_window_index_start[a] = samples_in_window_index_start[a] + 1
      if samples_in_window_index_start[a] >= len(signal):
        break_all_loop = True
        break
      if signal[samples_in_window_index_start[a]-1][a] < 1.65 and signal[samples_in_window_index_start[a]][a] >= 1.65:
        break
      if signal[samples_in_window_index_start[a]-1][a] > 1.65 and signal[samples_in_window_index_start[a]][a] <= 1.65:
        break
    if break_all_loop:
      break
    same_num_actual = (samples_in_window_index_start[a] - samples_in_window_index_start_last[a]) / samples_per_window
    same_num_diff = abs(same_num_actual - same_num)
    if same_num_diff > 0.7:
      print(f"a: {a}, same_num_actual: {same_num_actual:1.5f}, same_num: {same_num}")
      print(f"\033[31m")
      print(f"same_num_diff: {same_num_diff} > 0.7")
      print(f"\033[0m")
  if break_all_loop:
    break
  bit_num = bit_num + same_num
  bit_pool = bit_pool[same_num:]
  for a in range(bit_num_last+15, bit_num+15):
    bit_pool.append(transmit_bit(a))
continuous[0] = np.array(continuous[0], dtype=np.float64)
continuous[1] = np.array(continuous[1], dtype=np.float64)
continuous[2] = np.array(continuous[2], dtype=np.float64)
continuous[3] = np.array(continuous[3], dtype=np.float64)
continuous[4] = np.array(continuous[4], dtype=np.float64)
continuous[5] = np.array(continuous[5], dtype=np.float64)
continuous[6] = np.array(continuous[6], dtype=np.float64)
continuous[7] = np.array(continuous[7], dtype=np.float64)
continuous[8] = np.array(continuous[8], dtype=np.float64)
```

## 5. Generate Sample Continuous Bits Each Channel Eye-Diagram
```python
last_bit_gauge_color = ["orange", "red", "orange"]
for a in range(len(continuous)):
  if a == 0:
    x = np.arange(samples_per_1window_int)
    last_bit_gauge = [samples_per_window * 0.5, samples_per_window * 1.0, samples_per_window * 1.5]
  elif a == 1:
    x = np.arange(samples_per_2window_int)
    last_bit_gauge = [samples_per_window * 1.5, samples_per_window * 2.0, samples_per_window * 2.5]
  elif a == 2:
    x = np.arange(samples_per_3window_int)
    last_bit_gauge = [samples_per_window * 2.5, samples_per_window * 3.0, samples_per_window * 3.5]
  elif a == 3:
    x = np.arange(samples_per_4window_int)
    last_bit_gauge = [samples_per_window * 3.5, samples_per_window * 4.0, samples_per_window * 4.5]
  elif a == 4:
    x = np.arange(samples_per_5window_int)
    last_bit_gauge = [samples_per_window * 4.5, samples_per_window * 5.0, samples_per_window * 5.5]
  elif a == 5:
    x = np.arange(samples_per_6window_int)
    last_bit_gauge = [samples_per_window * 5.5, samples_per_window * 6.0, samples_per_window * 6.5]
  elif a == 6:
    x = np.arange(samples_per_7window_int)
    last_bit_gauge = [samples_per_window * 6.5, samples_per_window * 7.0, samples_per_window * 7.5]
  elif a == 7:
    x = np.arange(samples_per_8window_int)
    last_bit_gauge = [samples_per_window * 7.5, samples_per_window * 8.0, samples_per_window * 8.5]
  elif a == 8:
    x = np.arange(samples_per_9window_int)
    last_bit_gauge = [samples_per_window * 8.5, samples_per_window * 9.0, samples_per_window * 9.5]
  else:
    print(f"for a in range(len(continuous)): unexpected a value")
    exit(1)
  for b in range(len(continuous[a])):
    plt.figure()
    axes = plt.axes()
    axes.set_facecolor('black')
    for c in range(len(continuous[a][b])):
      plt.plot(x, continuous[a][b][c], color="blue", alpha=alpha)
    for c in range(len(last_bit_gauge)):
      plt.plot([last_bit_gauge[c], last_bit_gauge[c]], [-1.0, 4.0], color=last_bit_gauge_color[c], alpha=0.5)
    plt.gca().yaxis.set_major_formatter(plt.FormatStrFormatter('%.1f'))
    plt.ylim(-1.0, 4.0)
    plt.yticks([0.0, 3.3])
    plt.title(f"Eye Diagram of {bit_rate:.1e} bps")
    plt.xlabel("Samples")
    plt.ylabel("Voltage [V]")
    plt.savefig(f"alpha{alpha}_{name}_continuous{a+1}_ch{b}.jpg")
    plt.close()
```

## 6. Generate Sample Continuous Bits All Channel Eye-Diagram
```python
colors = ['orange', 'blue', 'green', 'white']
for a in range(len(continuous)):
  plt.figure()
  axes = plt.axes()
  axes.set_facecolor('black')
  if a == 0:
    x = np.arange(samples_per_1window_int)
    last_bit_gauge = [samples_per_window * 0.5, samples_per_window * 1.0, samples_per_window * 1.5]
  elif a == 1:
    x = np.arange(samples_per_2window_int)
    last_bit_gauge = [samples_per_window * 1.5, samples_per_window * 2.0, samples_per_window * 2.5]
  elif a == 2:
    x = np.arange(samples_per_3window_int)
    last_bit_gauge = [samples_per_window * 2.5, samples_per_window * 3.0, samples_per_window * 3.5]
  elif a == 3:
    x = np.arange(samples_per_4window_int)
    last_bit_gauge = [samples_per_window * 3.5, samples_per_window * 4.0, samples_per_window * 4.5]
  elif a == 4:
    x = np.arange(samples_per_5window_int)
    last_bit_gauge = [samples_per_window * 4.5, samples_per_window * 5.0, samples_per_window * 5.5]
  elif a == 5:
    x = np.arange(samples_per_6window_int)
    last_bit_gauge = [samples_per_window * 5.5, samples_per_window * 6.0, samples_per_window * 6.5]
  elif a == 6:
    x = np.arange(samples_per_7window_int)
    last_bit_gauge = [samples_per_window * 6.5, samples_per_window * 7.0, samples_per_window * 7.5]
  elif a == 7:
    x = np.arange(samples_per_8window_int)
    last_bit_gauge = [samples_per_window * 7.5, samples_per_window * 8.0, samples_per_window * 8.5]
  elif a == 8:
    x = np.arange(samples_per_9window_int)
    last_bit_gauge = [samples_per_window * 8.5, samples_per_window * 9.0, samples_per_window * 9.5]
  else:
    print(f"for a in range(len(continuous)): unexpected a value")
    exit(1)
  for b in range(len(continuous[a])):
    for c in range(len(continuous[a][b])):
      plt.plot(x, continuous[a][b][c], color=colors[b], alpha=alpha)
  for b in range(len(last_bit_gauge)):
    plt.plot([last_bit_gauge[b], last_bit_gauge[b]], [-1.0, 4.0], color=last_bit_gauge_color[b], alpha=0.5)
  plt.gca().yaxis.set_major_formatter(plt.FormatStrFormatter('%.1f'))
  plt.ylim(-1.0, 4.0)
  plt.yticks([0.0, 3.3])
  plt.title(f"Eye Diagram of {bit_rate:.1e} bps")
  plt.xlabel("Samples")
  plt.ylabel("Voltage [V]")
  plt.savefig(f"alpha{alpha}_{name}_continuous{a+1}.jpg")
  plt.close()
```

## 7. Makefile
```makefile
all:
        for f in $$(ls *.csv); do python3 main.py $$f $$(echo $$f | sed 's/-.*//g;s/tx//g'); done
```
