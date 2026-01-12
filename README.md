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

## Transient Analysis

### Netlist code
```
* Title: CR Ckt Simulation using SKY130 model
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
```
<img width="1835" height="1014" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/91da9e3b-6918-4b6c-9562-fed32773ff16" />



## Ac analysis
```
* RC circuit AC analysis
.lib "/home/sdash/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" tt
.temp 25

V1      in      0       AC 1
XR1     in      out     0       sky130_fd_pr__res_high_po_0p35  l=3.5
XC1     out     0       sky130_fd_pr__cap_mim_m3_1 w=1 l=1

* AC Simulation
.ac dec 10 1 15g

* Output commands
.control
run
.meas ac f3db WHEN VDB(out) = -3 ; –3 dB cutoff frequency
plot vdb(out)
.endc

.end
```
<img width="1805" height="1011" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/0dba199c-1988-4f87-b0de-1afff9ec59c3" />

## 2.MOSFET Circuits
-MOSFET (Metal–Oxide–Semiconductor Field-Effect Transistor) is a voltage-controlled device used for:

Switching (digital circuits)

Amplification (analog circuits)

👉 Current flows between Drain (D) and Source (S)
👉 Controlled by Gate (G) voltage
1️⃣ Cut-off Region (OFF)

V<sub>GS</sub> < V<sub>TH</sub>

No channel → No current

Used as open switch

2️⃣ Linear / Triode Region

V<sub>GS</sub> > V<sub>TH</sub>

Small V<sub>DS</sub>

Acts like a resistor

Used in analog switches

3️⃣ Saturation Region

V<sub>GS</sub> > V<sub>TH</sub>

V<sub>DS</sub> ≥ (V<sub>GS</sub> − V<sub>TH</sub>)

Used for amplification
## 3.Common Source Amplifier
```
************* common Source Amplifier with N-channel MOSFET and resistive load ************
*************Date:28/10/2025,Designer:ananya,BBSR *****************************************

.title CS Amplifier with NMOS driver and resistive load
.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xmn1 out in gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
Rd avdd out 8k
cl out gnd 10p

*vgs in gnd dc 0.9 ac -1
vsup avdd gnd dc 1.8
vin in gnd dc 0.9 ac 1 sin(0 1m 1000)
*.op
*.dc vgs 0 1.8 0.01
.ac dec 20 1 1G
*.tran 20u 1n

.control
run
set color0=white
print v(vout)
plot v(in) v(out)
plot db20(v(out)/v(in))
plot ph(v(out)/v(in))
.end
.endc
```


<img width="1836" height="1004" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/81863df7-54e0-4627-a451-6d2037cc6263" />
<img width="1839" height="1000" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/809bcf2c-3324-4299-8de1-37ef6215abd5" />



```
**********common source amplifiier with resistive load and source***************
*****************dc analysis************************
************Ananya abhijeeta,14/11/2025*************************
.title Source degenerated CS Amplifier with NMOS driver and resistive load

.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 out in Sn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rd vdd net1 8k
Rs Sn1 gnd 0.8k
Vcm net1 out dc 0
Cl out gnd 10p


vsup vdd gnd dc 1.8
vin in gnd dc 0.85 ac 1 sin(0.9 1m 100k)

*.dc vbn1 0 1.8 0.01
.dc vin 0 1.8 0.01

.control
run
set color0=white
plot v(out) v(sn1)
plot i(Vcm)
plot deriv(v(out))
.end
.endc
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e9f13ca5-0ae4-4536-b1a9-1a4f508b9557" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/96a89023-4016-43b4-85b5-0e45399d85b5" />




```
*********************Common source amp with N-channel mosfet*************************
*****************14/11/2025,ananya abhijeeta*********************
********************silicon university************

.title CS amplifier with NMoS Driver and PMOS current source load

.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 out in gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmn2 Dn2 Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmp1 Dp1 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6
xmp2 Dp2 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6
vcm1 Dp1 out dc 0
vcm2 Dp2 Dn2 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 0.9 ac 1 sin(0.9 1m 100k)
Vbn1 Gn2 gnd dc 0.9

*.dc Vbn1 0 1.8 0.01
.dc Vin 0 1.8 0.01

.control
run
set color0=white
plot v(out) v(Dp2)
plot i(Vcm1) i(Vcm2)
print abs(v(Dp2))
.end
.endc
```
<img width="1853" height="993" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/22d0c9c5-7265-44c9-9c54-eac4fa9e03ec" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8d91ef7a-ee39-4c5b-9c4e-7353cebfe31c" />

## Common Drain Amplifier


```
********************** Common Drain Amplifier with resistive load ************
******************************* AC ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Drain Amplifier With Resistive Load

.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rs out gnd 5k
*Rl Out Gnd 9
Vcm vdd Dn1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 1.5 ac 1 sin(1.438 1m 100k)

*.dc Vbn1 0 1.8 0.01
.ac dec 10 1 10G

.control
run
set color0=white
plot v(out)
plot ph(out)*(180/pi)
.end
.endc
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/939acf32-1913-4230-b71a-953ffada707c" />




```
********************* Common Drain Amplifier with MOSFET load ************
******************************* AC ANALYSIS ********************************
****************************** Date : 28/10/2025, ananya abhijeeta  *******************

.title Common Drain Amplifier With MOSFET Load

.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xm2 out Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
*Rl Out Gnd 10
Vcm vdd Dn1 dc 0
Cl out gnd 10p

Vsup vdd gnd dc 1.8
Vbias Gn2 gnd dc 0.85
Vin in gnd dc 1.5 ac 1 sin(1.8 1m 100k)

*.dc Vbn1 0 1.8 0.01
.dc Vin 0 1.8 0.01
.ac dec 10 1 1G

.control
run
set color0=white
plot v(out)
plot i(Vcm)
*plot ph((out)*180/pi)
plot deriv(v(out))
.end
.endc
```


<img width="1854" height="1014" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/28708bf6-4a99-4659-ad02-bdfd61eecf97" />
<img width="1829" height="1007" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/aa5f1247-d482-422f-bb60-96cfb5477e15" />

```
**********common source amplifiier with resistive load and source***************
*****************dc analysis************************
************Ananya abhijeeta,14/11/2025*************************
.title Source degenerated CS Amplifier with NMOS driver and resistive load

.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 out in Sn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rd vdd net1 8k
Rs Sn1 gnd 0.8k
Vcm net1 out dc 0
Cl out gnd 10p


vsup vdd gnd dc 1.8
vin in gnd dc 0.85 ac 1 sin(0.9 1m 100k)

*.dc vbn1 0 1.8 0.01
.dc vin 0 1.8 0.01 Rs 0 0.8k 0.2k

.control
run
set color0=white
plot v(out) v(sn1)
plot i(Vcm)
plot deriv(v(out))
.end
.endc
```
<img width="1853" height="990" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/64c94a26-6b83-4923-8175-51e0db995d9d" />
<img width="1826" height="1011" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/2923191f-1018-44fd-bc16-82c980a98867" />
<img width="1807" height="1031" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/1c662d65-5437-4a1b-a179-7c62ac68e42f" />


```
********************** Common Gate Amplifier with MOSFET and resistive load ************
******************************* DC ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Ananya Abhijeeta *******************

.title Common Gate Amplifier With MOSFET Load

.lib /home/ubuntu/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xm1 out Gn1 Sn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rd Rt1 out 8k
Vcm vdd Rt1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vgs Gn1 gnd dc 1.2
Vss Sn1 gnd dc 0.2 ac 1 sin(0.2 10m 1k)

.dc Vgs 0 1.8 0.01
*.ac dec 10 1 1G
*.tran 20u 1n

.control
run
set color0=white
plot i(Vcm)
plot v(Sn1) v(out)
*plot db(out)
*plot ph(out)*180/pi)
.end
.endc
```
<img width="1821" height="990" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/756d9e51-4acf-49f6-8d01-bcf43ed76cc9" />

















