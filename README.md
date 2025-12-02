# vfd-simulink-induction-motor
MATLAB/Simulink model of a three-phase VFD with V/f control for an induction motor, including PWM inverter, DC link, and speed control loop.

This repository contains a complete MATLAB/Simulink implementation of a Variable Frequency Drive (VFD) for a three-phase induction motor, using both open-loop V/f control and closed-loop speed control with a PI regulator.

<img width="1226" height="703" alt="Schéma global du modèle " src="https://github.com/user-attachments/assets/6a92232d-4d55-47de-ab5b-c254d833e65d" />


**The project models:**

PWM generation

MOSFET three-phase inverter

V/f law

Optional closed-loop speed controller

Load torque application using a repeating sequence

Motor electrical & mechanical behavior

This simulation is designed for students, engineers, and researchers working on drives, motor control, and power electronics.

## 1. System Command Chain 

<img width="1222" height="658" alt="Chaîne de commande" src="https://github.com/user-attachments/assets/49d5d76c-5eee-4118-bb06-92f4e38e52cd" />

The command chain includes:

Speed reference (step sequence or fixed reference)

PI controller (optional for closed-loop mode)

Integrator producing electrical angle

V/f scaling block

SPWM generator

## 2. V/f Control Implementation

The V/f block produces three sinusoidal reference waves (phase-shifted by 120°) whose amplitude scales linearly with frequency.

<img width="557" height="562" alt="vf" src="https://github.com/user-attachments/assets/ee9d20e8-2bba-4a1b-a828-383ef9be817a" />

Key functionalities:

Maintains constant flux at low and rated speed

Generates sin(2πft) for each phase A-B-C

Allows smooth motor startup

Acts as the fundamental of the PWM comparator

## 3. PI Speed Controller

<img width="886" height="282" alt="pi" src="https://github.com/user-attachments/assets/c23703b7-a4a0-4337-8d67-2eda42308458" />


Used only in closed-loop mode, the PI block:

Takes speed error = (speed_ref − measured_speed)

Outputs frequency command (Hz)

Ensures the motor follows the requested speed

Rejects disturbances (load torque variations)

When disconnected, the system runs open-loop using a fixed 50 Hz command.

## 4. MOSFET Inverter Architecture

<img width="901" height="633" alt="MOSFET" src="https://github.com/user-attachments/assets/fda9a6b0-5150-4f48-8f66-e5e1a297434a" />

The inverter uses six MOSFET switches:

S1–S2 → Phase A

S3–S4 → Phase B

S5–S6 → Phase C

Gate pulses:

[a][b] → top/bottom Phase A

[c][d] → top/bottom Phase B

[e][f] → top/bottom Phase C

Features:

High-frequency SPWM switching

Ideal MOSFETs

DC supply without rectifier (clean analysis of modulation)

## 5. Full Simulation Speed Response
5.1 Open-Loop Operation (PI disabled, fixed 50 Hz command)

<img width="700" height="627" alt="sans vfd" src="https://github.com/user-attachments/assets/a8ed26a4-9a74-4eb1-8f2a-c9d52d0c8b8e" />


Motor receives constant 50 Hz → expected speed ≈ 1430 rpm.

✔ Motor reaches steady speed
✔ Speed depends on slip and load
✔ No disturbance rejection

5.2 Closed-Loop Operation (PI enabled)

<img width="702" height="630" alt="BF" src="https://github.com/user-attachments/assets/81dfbfbb-9e86-4ae6-a84c-246cee49dd4f" />


Speed reference = 1430 rpm
Torque steps applied during the simulation

✔ Motor speed always returns to 1430 rpm
✔ PI compensates slip under load
✔ Excellent tracking and disturbance rejection

## 6. Load Simulation (Repeating Sequence Stair)

<img width="505" height="285" alt="charge" src="https://github.com/user-attachments/assets/cf86ab41-975f-4ef3-b135-0df201fe65cd" />


The Repeating Sequence Stair block is used to simulate changing mechanical load:

Multiple torque levels

Adjustable timing

Useful for testing PI robustness

Demonstrates speed recovery ability

## 7. How to Use the Model
Requirements

MATLAB R2021a or later

Simulink

Simscape Electrical (recommended)


**Steps**

Clone the repository

Open vfd_model.slx

Choose mode:

Open loop: disconnect PI → use fixed 50 Hz block

Closed loop: connect PI → command speed (rpm)

Adjust parameters:

DC voltage

Carrier frequency

PI gains

Load torque profile

Run the simulation

**Observe:**

Speed

Torque

PWM

Phase currents

Voltage waveforms

## 8. What You Can Learn from This Project

This repository helps you understand:

How a MOSFET inverter synthesizes AC from DC

How V/f control maintains motor flux

How slip affects speed and torque

Differences between open-loop and closed-loop control

How PI corrects steady-state error

How motors behave under load

How PWM waveform affects current and torque ripple
