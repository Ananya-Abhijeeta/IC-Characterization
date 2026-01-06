# IC-Characterization
This GitHub repository is created to provide hands-on experience in Analog IC Characterization using open-source tools such as NGSpice, Xschem, and the SKY130 PDK.

What is analog Characteriztion
Analog characterization is the process of measuring, simulating, and analyzing the performance parameters of an analog circuit or device (like a MOSFET, amplifier, op-amp, OTA) under different operating conditions.
Characterization is carried out using tools like Ngspice, Spectre, HSPICE, Eldo (Simulation) and Xschem (Schematic) providing a complete framework for accurate simulation and analysis of analog circuits.
Characterization is carried out using tools like Ngspice, Spectre, HSPICE, Eldo (Simulation) and Xschem (Schematic) providing a complete framework for accurate simulation and analysis of analog circuits

Contents

1.Linear elements
a)Resistors
A resistor is a passive electronic component that limits or controls the flow of current in a circuit by providing resistance (opposition to current).

👉 Unit: Ohm (Ω)
👉 Symbol: R

Ohm’s Law (Basic Rule)
V=I×R
Main Uses of Resistor

- To Limit Current
- To Divide Voltage,
- To Protect Components
  <img width="446" height="429" alt="image" src="https://github.com/user-attachments/assets/59d33acf-60a2-4d66-81f8-a511c5f21df4" />Title: Resistor Simulation

.lib "/home/sdash/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" tt  ; Load SKY130 PDK model at typical corner
.temp 25                                                                     ; Set simulation temperature to 25°C

Vin     in      0       DC 1.8        ; Apply 1.8V to the resistor input
Vm      in      1       0V            ; Zero-volt source used to measure current
X1      1       0       vdd   sky130_fd_pr__res_high_po_0p35 L=3.5   ; Resistor instance (high-poly) with L=3.5µm
vsup    vdd     gnd     DC 1.8        ; Body terminal of resistor tied to 1.8V supply

.op                                   ; Perform DC operating point analysis

.control
run                                   ; Run the simulation
print v(in)                           ; Display input voltage
print abs(i(Vm))                      ; Display current through resistor (via Vm)
let RES = v(in)/abs(i(Vm))           ; Calculate resistance using Ohm's Law
print RES                             ; Print calculated resistance
.endc

.end
  

