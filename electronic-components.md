# Electric Components

## 1. Bipolar Junction Transistor (BJT)
<div align="center"><img src="imgs/bipolar-junction-transistor.jpg" width="500"></div>

## 2. Phase-Locked Loop (PLL)
A Phase-Locked Loop (PLL) is a feedback control system that automatically adjusts the phase of a local oscillator to match the phase of an input signal.
It locks the phase of an output signal to a reference signal.

<div align="center"><img src="imgs/pll-block-diagram.jpg" width="500"></div>

### 2.1. Phase Detector (PD)
Compare the phase of the input and the VCO output.
 
### 2.2. Low-Pass Filter (LPF)
Filters the output of the PD to remove high-frequency noise.

### 2.3. Voltage-Controlled Oscillator (VCO)
Generates an output whose frequency is controlled by the voltage from the LPF.

### 2.4. Frequency Divider
Implemented in feedback path to allow frequency multiplication or division.

## 3. Capacitance
```math
{\Large
\begin{aligned}
Z_c &= \frac{1}{j \cdot 2\pi \cdot f \cdot C} \\
Z_c &: \rm{Impedance} \\
j &: \rm{Imaginary\ Unit} \\
f &: \rm{Input\ Frequency} \\
C &: \rm{Capacitance\ [F]} \\
\end{aligned}
} 
```

## 4. Inductance
```math
{\Large
\begin{align}
Z_L &= j \cdot 2\pi \cdot f \cdot L \\
Z_L &: \rm{Impedance} \\
j &: \rm{Imaginary\ Unit} \\
f &: \rm{Input\ Frequency} \\
L &: \rm{Inductance\ [H]} \\
\end{align}
}
```

## 5. Low-Pass RC-Circuit
> ### References
> [1] https://www.mathforengineers.com/transients-in-electrical-circuits/low-pass-RC-response-to-square-wave.html

<div align="center"><img src="imgs/low-pass-rc-circuit-diagram.jpg" width="500"></div>
