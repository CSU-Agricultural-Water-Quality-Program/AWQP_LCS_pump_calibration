### Pump Calibration for Low-Cost Water Sampler (LCS)

Overview  
This repository contains the **Pump Calibration Code** for the **Low-Cost Water Sampler (LCS)** developed by the CSU Agricultural Water Quality Program. This process calibrates the peristaltic pump by controlling a stepper motor and mapping motor steps to actual volume output.

---

### Requirements
#### Hardware
- Particle Boron LTE Microcontroller (e.g., BRN404)
- DRV8825 Stepper Motor Driver
- Peristaltic Stepper Pump
- 12V External Power Supply
- Breadboard or soldered perfboard
- Graduated cylinder or precision scale

#### Software
- [Particle Workbench](https://docs.particle.io/workbench/) or Arduino IDE
- [Particle CLI](https://docs.particle.io/tutorials/developer-tools/cli/) (optional)
- **Excel file: Pump Calibration Test.xlsx** (included in repository)

---

### Repository Structure
```bash
AWQP_LCS_pump_calibration/
├── src/
│   └── AWQP_pump_calibration.ino   # Main firmware script
├── lib/
│   └── AccelStepper/               # AccelStepper library
├── Pump Calibration Test.xlsx      # Spreadsheet for calibration
├── README.md                       # Project documentation
```

---

### Theory of Operation
This code drives a stepper motor using the AccelStepper library and a DRV8825 driver. The motor spins a peristaltic pump for a user-defined number of steps, and the output volume is measured manually. The resulting data are used to generate a calibration curve.

| Parameter       | Value   | Description                            |
|----------------|---------|----------------------------------------|
| `desiredSteps` | 10000   | Total steps per run                    |
| `desiredSpeed` | 1000    | Steps per second (motor speed)         |
| `timePause`    | 40000   | Pause between samples in milliseconds  |

---

### Calibration Instructions

1. **Connect Hardware**
   - `D3` → DRV8825 DIR pin  
   - `D5` → DRV8825 STEP pin  
   - `D2` → DRV8825 ENABLE pin  
   - 12V power to VMOT, shared GND with Boron

2. **Upload Firmware**
   - Flash `AWQP_pump_calibration.ino` to your Boron via USB or Particle OTA.

3. **Run Calibration**
   - Open the included **Pump Calibration Test.xlsx** file.
   - Set firmware to run **10,000 steps** and perform **10 trials**.
   - Record the **output volume in mL** for each trial.
   - Repeat the procedure in **10,000 step increments up to 50,000 steps**.
   - For each step count, calculate **mL/step**.
   - Use Excel to generate a **linear regression** line relating `steps` to `volume`.


4. **Apply the Equation**
   - Use the regression formula (e.g., `volume = mL/step × steps`) in your production firmware.
   
---

### Serial Output (Optional Debugging)
This firmware does **not** include serial output by default. To assist with debugging, you can add:

```cpp
Serial.begin(9600);
Serial.println("Pump cycle started");
```

---

### Troubleshooting
- **Pump not running**: Check ENABLE pin is LOW and wiring is correct.
- **Stepper jittering**: Try reducing speed or increasing current limit.
- **No volume output**: Check tubing, stepper coupling, and pump head integrity.

---

### Future Improvements
- Add cloud-connected volume logging using `Particle.publish()`
- Calibrate automatically with inline flow sensor (optional)
- Add CLI or app-based user input for `desiredSteps`

---

### References
- [DRV8825 Datasheet](https://www.ti.com/lit/ds/symlink/drv8825.pdf)
- [AccelStepper Library](http://www.airspayce.com/mikem/arduino/AccelStepper/)
- [Particle Boron Documentation](https://docs.particle.io/)

---

### License
This project is licensed under the **MIT License**.

Please contact the CSU Agricultural Water Quality Program for support or commercial licensing inquiries.

**Maintained by:** Manny Deleon, CSU AWQP  
**Last updated:** May 2025
