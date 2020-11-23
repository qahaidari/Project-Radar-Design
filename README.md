# Project-Radar-Design
Signal synthesis and processing in a radar design project

# Skills learned in this project:
* GIT
* FMCW radar system design
* Link budget preparation
* Transmit signal generation using Arbitrary Waveform Generator (AWG) Agilent
* Rohde & Schwarz RTO1044 oscilloscope for signal acquisition
* 2DFFT signal processing and simulation in MATLAB for target range and velocity estimation
* Optimization of module performance by using different parameter configuration
* Lab and site measurements
* Working with RF communications system components: PA, LNA, Mixer, Coupler, Rat-race coupler, LPF, Antenna

# Project Description

## Radar Signal Processing Chain

Below figure (Radar Design Project Slides by Tobias Chaloun, SS2019, Univeristy of Ulm) shows the block diagram of a typical radar system. I was involved in the signal synthesis and signal processing stages of the project.

![Block diagram](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/block%20diagram.PNG)

Below figure (Radar Design Project Slides by Tobias Chaloun, SS2019, Univeristy of Ulm) shows the signal processing chain.

![signal processing chain](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/signal%20processing%20chain.png)

For the signal synthesis stage, the following parameters are used:
Parameter | Value/Item
------------ | -------------
signal bandwidth | <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;5.6-5.9&space;GHz" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;5.6-5.9&space;GHz" title="5.6-5.9 GHz" /></a>
signal duration | <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;T_{chirp}&space;=&space;200&space;\mu&space;s" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;T_{chirp}&space;=&space;200&space;\mu&space;s" title="T_{chirp} = 200 \mu s" /></a>
signal generation in MATLAB | Sampling rate of AWG = <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;12&space;GHz" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;12&space;GHz" title="12 GHz" /></a>
Frequency-Time relationship of chirp | <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;f(t)&space;=&space;f_0&space;&plus;&space;(B/T_{chirp})&space;t" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;f(t)&space;=&space;f_0&space;&plus;&space;(B/T_{chirp})&space;t" title="f(t) = f_0 + (B/T_{chirp}) t" /></a>
Signal synthesis with AWG | Using AWG front panel software

For the signal acquisition/processing stage, the following parameters are used:
Parameter | Value/Item
------------ | -------------
RTO sampling rate | <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;100&space;MHz" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;100&space;MHz" title="100 MHz" /></a>
RTO sampling time | <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;N_{chirp}&space;*&space;T_{chirp}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;N_{chirp}&space;*&space;T_{chirp}" title="N_{chirp} * T_{chirp}" /></a>
RTO scaling parameter | to be adjusted accordingly
signal processing in MATLAB | 1D-FFT, 2D-FFT, Filtering, Windowing, Zero Padding
signal evaluation | range, velocity, RV plots

## Signal Synthesis using AWG

The figure shows the AWG used in this project for generating the transmit power.

![AWG](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/AWG.jpg)

AWG can produce three different power modes. We can use the output port which has an internal amplifier and therefore generate a high power level. We set the voltage amplitude based on the link budget of the project. As an example: if based on the link budget, we need 10dBm power, then the peak-peak voltage level can be calculated from the following equations:

<a href="https://www.codecogs.com/eqnedit.php?latex=V_{rms}&space;=&space;\frac{V_p}{\sqrt{2}}&space;=&space;\frac{V_{pp}}{2\sqrt{2}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?V_{rms}&space;=&space;\frac{V_p}{\sqrt{2}}&space;=&space;\frac{V_{pp}}{2\sqrt{2}}" title="V_{rms} = \frac{V_p}{\sqrt{2}} = \frac{V_{pp}}{2\sqrt{2}}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=P_{avg}&space;=&space;\frac{(V_{rms})^2}{R}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P_{avg}&space;=&space;\frac{(V_{rms})^2}{R}" title="P_{avg} = \frac{(V_{rms})^2}{R}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=10dBm&space;=&space;-20dBW&space;=&space;10&space;\log_{10}\frac{(\frac{V_{pp}}{2\sqrt{2}})^2}{50}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?10dBm&space;=&space;-20dBW&space;=&space;10&space;\log_{10}\frac{(\frac{V_{pp}}{2\sqrt{2}})^2}{50}" title="10dBm = -20dBW = 10 \log_{10}\frac{(\frac{V_{pp}}{2\sqrt{2}})^2}{50}" /></a>

In the above equations, a <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;50\Omega" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;50\Omega" title="50\Omega" /></a> input impedance is considered for the RTO oscilloscope.

Based on the required voltage level, the AWG front panel software is then used to set the configurations for signal generation. Different parameters for signal generation can also be set in MATLAB and then sent to AWG via LAN/PCIe cable. Below figures show the AWG front panel software and the corresponsing configuration settings.

![AWG frontpanel1](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/AWG%20frontpanel_1.jpg)
![AWG frontpanel2](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/AWG%20frontpanel_2.jpg)
![AWG frontpanel3](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/AWG%20frontpanel_3.jpg)

## Signal Acquisition using RTO

The figure shows the RTO oscilloscope used in this project for received radar signal acquisition and sampling.

![RTO](https://github.com/qahaidari/Project-Radar-Design/blob/main/images/RTO.jpg)

RTO samples IF signal from the mixer outputs. A trigger cable for synchronization between AWG and RTO is used. MATLAB is used for signal generation to AWG and processing the samples received from RTO.






