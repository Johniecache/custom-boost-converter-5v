# Calculations
### Assumptions
 * FB and the GND pin (R2) is 100k&Omega;
 * SOT23-6 MT3608 frequency at 1.2MHz
 * Common Resistors: 100&Omega;, 220&Omega;, 330&Omega;, 1k&Omega;, 2k&Omega;, 5.1k&Omega; 10k&Omega;, 100k&Omega;, 1M&Omega;


### Inductor & Ripple
$$
V_{out} = \frac{V_{in}}{1 - D} \rightarrow D = 1 - \frac{V_{in}}{V_{out}} \rightarrow 1 - \frac{3.7v}{5v} \approx 0.26
$$

$$
&Delta;IL = \frac{V_{in} * D}{L * f} \rightarrow \frac{(3.7v)(0.26)}{(33 * 10^{-6}H)(1.2 * 10^{6}Hz)} = 24.3mA
$$



### GND to Vin Capacitor
$$
C_{in} = \frac{I_{in} * D}{&Delta;V_{in} * f} \rightarrow \frac{(1A)(0.26)}{(0.1v)(1.2*10^{6}Hz)} \approx 2.17&mu;F
$$
 * 0.1v is chosen because it is one of the more efficient values for a stable, high preformance, and long lasting maximum input ripple
 * The reason for a 10&mu;F is because it allows for headroom for the transient and takes into account aging under voltage. The true value of this is more of roughly 6&mu;F

### GND to Vout Capacitor
$$
C_{out} = \frac{I_{out} * D}{f * &Delta;V_{out}} \rightarrow \frac{(0.5A)(0.26)}{(1.2*10^{6}Hz)(0.1v)} = 1.08&mu;F
$$
 * 100&mu;F chosen because:
   * Compensates for Equivalen Series Resistance (ESR) and provide margin
   * Avoid brownout incase of sudden plug-ins
   * MT3608 expects roughly 10-22&mu;F so taking a 100&mu;F improves stability without risking oscillation
   * Cheap and very common
   * Keeps ripples below or at 100mA at 0.5 - 1 A loads

### Feedback Resistor
$$
V_{out} = V_{ref}(1 + \frac{R1}{R2})
$$
 * R1 is between the $V_{out}$ pin and the FB pin
 * R2 is between the FB and the GND pin (assume this to be 100k&Omega;)

$$
R1 = R2 (\frac{V_{out}}{V_{ref}} - 1) \rightarrow 100k(\frac{5v}{0.6v} - 1) = 733.34k&Omega;
$$

Quick check:

$$
0.6v(1 + \frac{733.34k&Omega;}{100k&Omega;}) \approx 5.00v
$$

### Optimization of the Feedback Resistor
$$
R_{p1} = (\frac{1}{1M&Omega;} + \frac{1}{1M&Omega;})^-1 = 500k&Omega;
$$
$$
R_{p2} = (\frac{1}{100k&Omega;} + \frac{1}{100k&Omega;} + \frac{1}{100k&Omega;})^-1 = 33.34k&Omega;
$$
$$
R_{t} = 500k&Omega; + 33.34k&Omega; + 200k&Omega; = 733.34k&Omega;
$$

