# Processor in loop (PIL) tests on a Virtual Cortex - M7 processor 
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MATLAB Compatibility](https://img.shields.io/badge/MATLAB-R2026a-blue.svg)](https://www.mathworks.com/products/matlab.html)
[![Status](https://img.shields.io/badge/Status-Ongoing-success.svg)]()
> This project demonstrates a full model-based development workflow for an automotive control system. Starting from a prebuilt Simulink controller, it defines software requirements, creates test scenarios, generates production-style C code, and executes Processor-in-the-Loop tests on a virtual Arm Cortex-M7. The workflow verifies functional correctness, timing performance, and traceability, culminating in an automated verification report suitable for safety-critical development processes.

>## 📜 Table of Contents 
* Selction of the Application & defining high level requirements
* Modification of Controller
* Model-in-the-Loop (MIL) Test Cases
* Preparation of the Controller for Code Generation
* Configuration of code for Cortex-M7
* Run Processor-in-the-Loop (PIL) Tests
* Publishing the Verification Pack

### Selction of the Application & defining high level requirements

Now for my Project I have slected the ABS Anti Lock braking system as my working built in model in Simulink . An anti-lock braking system (ABS) is a vehicle safety feature that prevents wheels from locking up and skidding during hard braking, allowing the driver to maintain steering control. It does this by using sensors to monitor wheel speed and a control module to rapidly pulse the brakes, reducing pressure to a wheel that is about to lock, and then reapplying it. This allows the tires to maintain traction, which helps the driver steer to safety even in an emergency stop.

Wheel locking during breaking isn't just a minor inconvenience, it's a physics nightmare. When brakes are applied too aggressively on slippery surfaces, friction between the tire and road drops to near zero, causing the wheel to skid rather than rotate. This phenomenon, governed by the tire's friction circle, leads to a coefficient of friction (μ) plummeting from 0.8-1.0 (dry asphalt) to as low as 0.1 (ice), extending stopping distances by up to 30-50% and stripping directional control.

For developers, consider the dynamics: Newton's second law (F = μN) dictates that locked wheels dissipate kinetic energy inefficiently, converting it into uncontrolled sliding. Historical data from NHTSA crash reports highlights that skidding contributes to 20% of fatal accidents in adverse conditions. Enter the Anti-Lock Brake System (ABS) Control Module: by cyclically modulating brake pressure, it keeps wheels at the peak of the μ-slip curve (typically 10-20% slip ratio), preserving steerability. This context underscores the ABS ECU's role—not as a luxury, but as a life-saving imperative in modern automotive ECUs.

Now going on further we define the high level requirements 

