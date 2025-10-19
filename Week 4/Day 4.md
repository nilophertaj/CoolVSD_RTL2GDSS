# CMOS Noise Margin Robust Evaluation

**Noise Margin (NM)** is a measure of a digital circuit's tolerance to noise. It defines the maximum voltage noise that can be tolerated without causing a logic error.

## Definitions

- **Noise Margin High (NMH):**
  \[
  NMH = V_{OH} - V_{IH}
  \]

- **Noise Margin Low (NML):**
  \[
  NML = V_{IL} - V_{OL}
  \]

Where:  
- \(V_{OH}\) = Output High Voltage  
- \(V_{OL}\) = Output Low Voltage  
- \(V_{IH}\) = Input High Voltage (minimum recognized as HIGH)  
- \(V_{IL}\) = Input Low Voltage (maximum recognized as LOW)  

## Robust Evaluation

1. **Static Noise Margin (SNM):**  
   - Evaluated using the **butterfly curve** of CMOS inverter.  
   - SNM is the side length of the largest square that fits between the two voltage transfer curves.  

2. **Factors Affecting NM Robustness:**  
   - **Process variations** (threshold voltage, channel length)  
   - **Temperature variations**  
   - **Supply voltage variations**  

3. **Improving NM Robustness:**  
   - Using **transistor sizing** to adjust pull-up/pull-down strengths.  
   - Optimizing **VTC (Voltage Transfer Characteristic)** symmetry.  
   - Minimizing **leakage currents** and **power supply noise**.  

> **Summary:**  
> A robust CMOS design ensures NMH and NML are high enough to tolerate expected noise, process, voltage, and temperature variations, improving circuit reliability.
