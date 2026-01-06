# IC-Characterization
This GitHub repository is created to provide hands-on experience in Analog IC Characterization using open-source tools such as NGSpice, Xschem, and the SKY130 PDK.

What is analog Characteriztion
Analog characterization is the process of measuring, simulating, and analyzing the performance parameters of an analog circuit or device (like a MOSFET, amplifier, op-amp, OTA) under different operating conditions.
Characterization is carried out using tools like Ngspice, Spectre, HSPICE, Eldo (Simulation) and Xschem (Schematic) providing a complete framework for accurate simulation and analysis of analog circuits.
Characterization is carried out using tools like Ngspice, Spectre, HSPICE, Eldo (Simulation) and Xschem (Schematic) providing a complete framework for accurate simulation and analysis of analog circuits

Contents

Linear elements

1.Resistors

A resistor is a passive electronic component that limits or controls the flow of current in a circuit by providing resistance (opposition to current).

👉 Unit: Ohm (Ω)
👉 Symbol: R

Ohm’s Law (Basic Rule)
V=I×R
Main Uses of Resistor

- To Limit Current
- To Divide Voltage,
- To Protect Components
<img width="446" height="429" alt="image" src="https://github.com/user-attachments/assets/27cac24b-fa26-4abf-ade2-cf95eb2d9bf0" />

2.Capacitors
- A capacitor is a passive electronic component that stores electrical energy in the form of an electric field between two conductive plates separated by an insulating material called a dielectric.
- The capacitance C of a parallel-plate capacitor depends on its physical structure and the material between the plates, given by the formula: C = εA / d
- 👉 Unit: Farad (F)
👉 Symbol: C.
<img width="510" height="401" alt="image" src="https://github.com/user-attachments/assets/3893a322-1b51-4fb7-a5ce-c169544d620f" />

3.RC Circuit
- An RC circuit is an electric circuit composed of resistors (R) and capacitors (C), which exhibit a time-dependent response to voltage or current changes. The fundamental time constant is defined as:
τ = R * C, where τ (tau) represents the time constant in seconds, indicating how quickly the circuit charges or discharges.
<img width="539" height="429" alt="image" src="https://github.com/user-attachments/assets/ec3d6e55-4a4f-449b-a3c5-86a8fc855fde" />

# Transient Analysis

## Netlist code
`` * Title: CR Ckt Simulation using SKY130 model
.lib "/home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt"
.temp 25

Vin     in      0       PULSE(0 1.8 0 0 0 100p 200p)
XC1     in      out     sky130_fd_pr__cap_mim_m3_1 w=1 l=1
XR1     out     0       0       sky130_fd_pr__res_high_po_0p35 l =3.5

.tran 1p 300p

.control
run
plot v(in) v(out)
.endc

*Measure Time delays
.meas tran rise TRIG V(out) VAL=0.14 RISE=1 TARG V(out) VAL=1.29 RISE=1
.meas tran fall TRIG V(out) VAL=1.29 FALL=1 TARG V(out) VAL=0.14 FALL=1
.meas tran rise_delay TRIG V(in) VAL=0.7 RISE=1 TARG V(out) VAL=0.7 RISE=1
.meas tran fall_delay TRIG V(in) VAL=0.7 FALL=1 TARG V(out) VAL=0.7 FALL=1

*Measure Max Voltage
.meas tran VMAX MAX V(out)

.end
``
<img width="1907" height="782" alt="image" src="https://github.com/user-attachments/assets/b5f392bf-dfd9-48fd-81f4-04db5a20dc85" />







