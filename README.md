# Bandgap_Reference_Circuit_Using_ASA_7nm_FinFET_PDK

## Scaling Beyond CMOS: FinFET Devices and Innovations

### Evolution of Integrated Circuits

In the beginning, around World War 2, Alan Turing and his colleagues invented an application-specific mechanical machine called Bombe to break the engima code. This laid the foundation for Integrated Circuits. In the post war era, inventions such as the ENIAC and EDVAC were invented for large-scale simulations. These machines used vaccum tubes to carry out the simulations. The drawback for these kind of machines was its size, as it stretched from the floor to ceiling and occupied entire rooms. Looking at the modern systems, we can fit the entire ENIAC machine on a chip sightly bigger than a coin. The next-generation chips will be tackling problems such as weather modelling and forecasting, health and precision medicine, virtual particle accelerator and many more.

<img width="834" height="467" alt="image" src="https://github.com/user-attachments/assets/fbf4da29-f75f-4bac-bf2e-ccb1a4c6d782" />

### Microprocessor Trend
If we look at the data from the past 50 years, we can see that the Moore's Law still stands as the number of transistors on a chip are doubling every 18-20 months. Even though the transistors are doubling, the performance of the chips have started to saturate. This saturation is due to our inability to cool down the systems at a faster rate. This leads to saturation in power and frequency of the chips. Among all this, we can see that the number of cores on a chip is gradually increasing. These advancements took place as a result of iPhone being released. The mobile industry bought along numerous mobile applications which resulted in increase in number of cores.

<img width="838" height="471" alt="image" src="https://github.com/user-attachments/assets/9418a5e6-7782-44ed-9a2d-a4a174a337cd" />

For solving larger and larger problems, we will see a shift from indiviual devices to data-centre scale computing. In a data-centre, racks will be used as a socket. A rack contains many computing elements. In this scale, we use flop(floating point operations per second) to measure its performance. Currently, we have attained 8-10 Exaflop performance. The trend suggests that the flop performance doubles every 2 years. But, with increase in flops, the system demands more power. 

<img width="836" height="469" alt="image" src="https://github.com/user-attachments/assets/69865c74-6718-4c8f-b671-98c671aed3f9" />

### CMOS Evolution an Next-Gen Candidates
<img width="839" height="468" alt="image" src="https://github.com/user-attachments/assets/b48235c3-464d-4dd1-bff9-cb48d0aad5be" />

The evolution of CMOS circuits is travelling towards 1nm technology node. The given image shows the roadmap of CMOS evolution. The Moore's Law has been essential to the semiconducter industry, but as the dimension of the devices reduces, simple lithography shrinks have become more challenging. To overcome this shortcomings,research is being conducted in several areas such as chennel materials, patterning methods, etc.

New channel materials such as Germanium, SiGe, and 2D materials such as MoS2 are being developed to replace conventional materials like Si and Ge. Moreover, patterning techniques like EUV, High-NA EUV have been reseached to increase the number of transistors on a chip. There also have been advancements in inteconnection material, gate stack material as well as the device structure which looks into various structures such as MBC-FET(Multi-Bridge Channel FET), 3DS-FET(3D Stacked FETs) and many more.

## Fin-FETs
A FinFET is a type of Field Effect Transistor which uses thin, fin-like silicon body that protrudes vertically from the substrate instead of a planar design. The gate wraps around the fin which gives it better control over the channel.

<img width="837" height="469" alt="image" src="https://github.com/user-attachments/assets/ae0040d3-31f4-4226-8574-bf90ba26c70c" />

FinFETs usually use 22nm or less technology node. In planar structure FETs, there was a possibility of sub-channel leakages, but if we include a gate on both sides of the channel, the sub-channel leakage diminishes. Two gates also enhances the gate capacitance as well as the short-channel performance. Also, the FinFET offer near to ideal sub-threshold swing.  

## 7nm FinFET Inverter Performance Analysis
In this segment, we will analyze the performance of the 7nm FinFET inverter thriugh metrics Switching Threshold Voltage (VTC), Drain Current (Id), Power Consumption (P), Propagation Delay (tpd), Gain (Av), Noise Margin (NM), Transconductance (gm), Frequency (f). We will be using xschem software to analyze the spice file of the structure. To analyze it efficiently, we will be analyzing the structure for various W/L ratio. For unique and traceable simulation, we will be adding a dummy voltage in the spice deck. 

The spice file of the fin_fet used in the inverter is given as
```
** sch_path: /home/hprcse/Finfet/nfet_char.sch
**.subckt nfet_char
V1 nfet_in GND 0
V2 vdd GND 3
Vuniq in 0 DC 0.428
R1 vdd nfet_out 1k m=1
Xnfet2 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=14
**** begin user architecture code


.dc v1 0 0.7 1m v2 0 0.7 0.2
.control
run
set xbrushwidth=3
let vd = vdd - nfet_out
let id  = vd/1000
plot id
.endc


**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_nfet S G D B l=7e-009 nfin=14
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=14
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/vsduser/Desktop/asap_7nm_Xschem/bsimcmg.osdi
.endc


**** end user architecture code
.end

```

The transfer characterisitics and the output characteristics of the fin_fet are given below

Transfer Characteristics

<img width="704" height="552" alt="image" src="https://github.com/user-attachments/assets/8dfdad8d-b2fe-46b4-8d3e-7e13f5e6f3dd" />

Output Characteristics

<img width="702" height="548" alt="image" src="https://github.com/user-attachments/assets/c8730aaa-f249-4ccc-a6bd-1457e40c1f1b" />

Now that we have analyzed the nfet, we can move on to the analysis of inverter circuit using the finfet. WE will be using offset in the pulse voltage to obtain unique results.

The offset voltage value is derived from adding the ASCII values of my name (amey = 97 + 109 + 101 + 121 = 428)
To add the offset voltage, we will use the following command,
```
V1 nfet_in GND pulse(0.428 1.128 20p 10p 10p 20p 500p 1)
```


The spice deck used for the inverter analysis is given below:
```

** sch_path: /home/vsduser/Desktop/asap_7nm_Xschem/inverter_vtc2.sch
**.subckt inverter_vtc
Xpfet1 nfet_out nfet_in vdd vdd asap_7nm_pfet l=7e-009 nfin=14
Xnfet1 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=14
V1 nfet_in GND pulse(0.428 1.128 20p 10p 10p 20p 500p 1)
V2 vdd GND 0.7

**** begin user architecture code


.dc v1 0 0.7 1m
*.tran 1e-12 100e-12

.control
    * First run DC
    dc v1 0.428 1.128 1m
    run

    * DC measurements
    meas dc v_th when nfet_out = nfet_in
    plot nfet_out nfet_in
    
    let gain_av = abs(deriv(nfet_out))
    meas dc max_gain max gain_av
    let gain_target = max_gain * 0.999
    meas dc vil find nfet_in when gain_av = gain_target cross=1
    meas dc voh find nfet_out when gain_av = gain_target cross=1
    meas dc vih find nfet_in when gain_av = gain_target cross=2
    meas dc vol find nfet_out when gain_av = gain_target cross=2
    let nmh = voh - vih
    let nml = vil - vol
    print v_th max_gain vil voh vih vol nmh nml
    
    *Transconductance
    
    let id = v2#branch
    let gm = real(deriv(id, nfet_in))
    meas dc gm_max MAX gm
    plot gm
    let r_out= deriv(nfet_out,id)
    plot r_out
    plot id
    
    * Transient measurements
    tran 1p 100p
    meas tran tpr when nfet_in = 0.778 rise = 1
    meas tran tpf when nfet_out = 0.035 fall = 1
    plot tpr
    plot tpf
    plot v(nfet_in) v(nfet_out)
    let tp = (tpr + tpf) / 2
    let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=2e-11 to=6e-11
    let pwr = id_pwr * 0.7
    let power = abs(pwr / 40e-12)
    print tpr tpf tp id_pwr pwr power
  
    tran 1p 100p                         
    meas tran tr when nfet_in=0.498 RISE=1  
    meas tran tf when nfet_out=0.071 FALL=1 
    let t_delay = tr + tf                  
    print t_delay                         
    let f = 1/t_delay                     
    print f                              

    

.endc




**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_pfet S G D B l=7e-009 nfin=nfin
	npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=nfin
.ends asap_7nm_pfet

.model BSIMCMG_osdi_P BSIMCMG_va (
+ TYPE = 0

************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 3e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.9278          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2.5e-009       dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.8e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.003469        cdscd   = 0.001486        dvt0    = 0.05
+dvt1    = 0.36            phin    = 0.05            eta0    = 0.094           dsub    = 0.24
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 0
+etaqm   = 0.54            qm0     = 2.183e-012      pqm     = 0.66            u0      = 0.0237
+etamob  = 4               up      = 0               ua      = 1.133           eu      = 0.05
+ud      = 0.0105          ucs     = 0.2672          rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 60000           deltavsat= 0.17            ksativ  = 1.592
+mexp    = 2.491           ptwg    = 25              pclm    = 0.01            pclmg   = 1
+pdibl1  = 800             pdibl2  = 0.005704        drout   = 4.97            pvag    = 200
+fpitch  = 2.7e-008        rth0    = 0.15            cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 0               lcdscdr = 0               lrdsw   = 1.3             lvsat   = 1441
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.007           bigc    = 0.0015          cigc    = 1               dlcigs  = 5e-009
+dlcigd  = 5e-009          aigs    = 0.006           aigd    = 0.006           bigs    = 0.001944
+bigd    = 0.001944        cigs    = 1               cigd    = 1               poxedge = 1.152
+agidl   = 2e-012          agisl   = 2e-012          bgidl   = 1.5e+008        bgisl   = 1.5e+008
+egidl   = 1.142           egisl   = 1.142
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -1.2            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/vsduser/Desktop/asap_7nm_Xschem/bsimcmg.osdi
.endc



.subckt asap_7nm_nfet S G D B l=7e-009 nfin=14
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=14
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/vsduser/Desktop/asap_7nm_Xschem/bsimcmg.osdi
.endc


**** end user architecture code
.end
```


To carry out the simulation, the following command was used:


```
#Open the directory in which the spice file is present

cd asap_7nm_Xschem

#run the spice deck

ngspice inverter_vtc2.spice
```

We get the following plots for VTC, Id, and delay

Drain Current

<img width="704" height="546" alt="1" src="https://github.com/user-attachments/assets/bdc74307-b313-4f9f-825a-0497976ffa70" />

Output Resistance

<img width="706" height="550" alt="2" src="https://github.com/user-attachments/assets/174aac79-f4ce-42d8-8a9b-338b4c454019" />

Transconductance

<img width="707" height="546" alt="3" src="https://github.com/user-attachments/assets/b1c9a9c4-ada6-41c8-81b4-fe30d160d35f" />

Voltage Transfer Characteristics

<img width="703" height="544" alt="4" src="https://github.com/user-attachments/assets/df062243-6a1e-4e76-8f5f-a344412e043d" />

The parameter outputs for W/L = 2

<img width="556" height="398" alt="5" src="https://github.com/user-attachments/assets/72d33f1a-262b-4dc8-8d2d-2d6139b11a57" />

<img width="772" height="820" alt="6" src="https://github.com/user-attachments/assets/57d404f6-076d-4994-98f0-e3f13ef2058e" />


To analyze the performance at different W/L ratios, we need to create a characterization table
To change the W/L ratio, we need to change the nfin value in the following commands
```
Xpfet1 nfet_out nfet_in vdd vdd asap_7nm_pfet l=7e-009 nfin=14

asap_7nm_pfet S G D B l=7e-009 nfin=nfin
npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=nfin
```

<img width="753" height="177" alt="image" src="https://github.com/user-attachments/assets/72737c81-e350-451a-be99-c8b0e947b6b2" />


## Bandgap Reference Design and Simulation using Xschem

Now, that we have done the performance analysis of the FinFET Inverter, we can implement it into a circuit. We will be implementing a Bandgap Refernece Circuit Dsign using the FinFET inverter. 

The below photo is taken as the reference circuit for designing the Bandgap reference.

<img width="822" height="518" alt="image" src="https://github.com/user-attachments/assets/541a7e5b-ebca-49f8-ac2f-372eef6dbcf7" />

### Steps to implement the circuit in Xschem

1. Open Xschem
```
Xschem
```
2. Press ``` ctrl + I ``` to ivoke the component list.
3. Search pfet and nfet and select them indivually and drag them onto the board to create the circuit.
4. To get multiple nfet of same properties, you can duplicate the nfet/pfet.
5. Create the circuit
6. Make connections as given in the schematic.
7. Search for VDD, GND, source, resistance in the component list and connect them in their respective place.
8. Add a simulation deck command window
9. Add the commands to plot various desired graphs.

Code for dc analysis used to plot Vref and VCTAT
```
name=s1 only_toplevel=false value="
.dc temp -45 150 5
.dc VDD 0 1 0.1
.control
pre_osdi /home/vsduser/Desktop/asap_7nm_Xschem/bsimcmg.osdi
run
plot v(Vref) v(VCTAT)
plot v(Vref)-v(VCTAT)
plot v(VCTAT)
plot v(Vref)
let temp_coeff = deriv(v(Vref))/1.24
plot temp_coeff
plot net9/30k Vref/33.33k VCTAT/33.33k
plot abs(v2#branch)
.endc
"
```

Code for transient analysis used to plot the startup time
```
name=s1 only_toplevel=false value="
.tran 0.01p 100p
.temp -45
.control
pre_osdi /home/vsduser/Desktop/asap_7nm_Xschem/bsimcmg.osdi
run

plot v(VCTAT)
plot v(Vref)

.endc
"


```
