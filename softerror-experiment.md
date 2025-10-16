# Soft-Error Experiment

## 1. Get Report Using UART
```python
import sys
import serial
import datetime

if len(sys.argv) >= 2:
  note = sys.argv[1]
else:
  note = ""

try:
  tty = serial.Serial(port='/dev/ttyUSB0', baudrate=115200, timeout=1)
  timestamp = datetime.datetime.now().strftime("%Y%m%d%H%M")
  print(f"Opening {timestamp}{note}.log")
  sorted_port = []
  sorted_address = []
  sorted_count = []
  total_count = 0
  with open(f"{timestamp}{note}.log", "w") as flog:
    with open(f"{timestamp}{note}.raw.log", "w") as frawlog:
      while True:
        data = tty.readline()
        if data:
          text = data.decode().strip()
          frawlog.write(f"{text}\n")
          (port, address, count) = text.split(":")
          port = int(port, 16)
          address = int(address, 16)
          count = int(count, 16)
          total_count = total_count + count
          print(f"{port}-{address}: {count}")
          flog.write(f"{port}-{address}: {count}\n")
          if len(sorted_count) == 0:
            sorted_port.append(port)
            sorted_address.append(address)
            sorted_count.append(count)
          else:
            for a in range(len(sorted_count)):
              if count > sorted_count[a]:
                sorted_port.insert(a, port)
                sorted_address.insert(a, address)
                sorted_count.insert(a, count)
                break
              elif a == len(sorted_count) - 1:
                sorted_port.append(port)
                sorted_address.append(address)
                sorted_count.append(count)
          if port == 6 and address == 127:
            break
    print(f"Total Count: {total_count}")
    flog.write(f"Total Count: {total_count}\n")
    for a in range(10):
      print(f"Rank: {a+1}, Port: {sorted_port[a]}, Address: {sorted_address[a]}, Count: {sorted_count[a]}")
      flog.write(f"Rank: {a+1}, Port: {sorted_port[a]}, Address: {sorted_address[a]}, Count: {sorted_count[a]}\n")
except KeyboardInterrupt:
  print("\nInterrupted")
finally:
  if 'tty' in locals() and tty.is_open:
    print("\n/dev/ttyUSB0 Closed")
    tty.close()
```
