# PLL Generation Script

```bash
CLOCK_FREQUENCY="10MHz"

ip-generate \
  --component-name="altera_pll" \
  --system-info=DEVICE_FAMILY="Cyclone V" \
  --system-info=DEVICE="5CSEMA5F31C6" \
  --output-directory="${PWD}" \
  --file-set="QUARTUS_SYNTH" \
  --output-name="GeneratedPLL" \
  --component-param=gui_reference_clock_frequency="50MHz" \
  --component-param=gui_output_clock_frequency0="${CLOCK_FREQUENCY}" \
  --component-param=gui_duty_cycle0="50%" \
  --component-param=gui_phase_shift0="0" \
  --allow-mixed-language-simulation \
  --language="vhdl" 
```

