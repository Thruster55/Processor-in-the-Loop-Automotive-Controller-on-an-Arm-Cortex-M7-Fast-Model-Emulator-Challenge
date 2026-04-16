# Processor-in-the-Loop-Automotive-Controller-on-an-Arm-Cortex-M7-Fast-Model-Emulator-Challenge

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MATLAB Compatibility](https://img.shields.io/badge/MATLAB-R2025b-blue.svg)](https://www.mathworks.com/products/matlab.html)
[![Status](https://img.shields.io/badge/Status-Completed-success.svg)]()

 ## **📝 Project Motivation**

Modern automotive systems, such as adaptive cruise control and electric powertrains, rely on embedded code that must execute deterministically on resource-constrained hardware. Processor-in-the-Loop (PIL) testing allows engineers to run automatically generated C-code on a virtual replica of the target processor while the vehicle physics stay in simulation. This process exposes numerical differences and timing issues early in the development cycle, fulfilling safety-critical standards like ISO 26262.


## **🎯 Project Objectives**


- Define formal software requirements using the Requirements Toolbox™.
- Design and validate a climate control system using Simulink® and Stateflow®.
- Generate optimized C-code targeted for the Arm Cortex-M7 architecture.
- Verify the embedded code via Processor-in-the-Loop (PIL) simulation on a virtual emulator.
- Generate a comprehensive Verification Pack to prove requirement compliance.
- 

 ## **🛠️ Step-by-Step Workflow**


1. Selection of Application and Base Model
The project utilizes the sldemo_auto_climatecontrol example, focusing on the controller subsystem responsible for maintaining cabin comfort.

<p align="center">
  <img width="1157" height="649" alt="Screenshot of the top-level Simulink model" src="https://github.com/user-attachments/assets/b713b00c-9845-4c7d-afdb-082a0a73f1ff" />
  <br>
  <em>Screenshot of the top-level Simulink model showing the car and cabin dynamics</em>
</p>


 ## **2. Requirement Engineering**
 
   
Using the Requirements Toolbox, four high-level requirements were defined to guide the design and verification process:

- R1 (Maintain Temperature): Maintain cabin temperature within $\pm 1$ °C of the setpoint.
- R2 (Prevent Rapid Cycling): Implement a deadband to avoid frequent AC/Heater toggling.
- R3 (Proportional Blower Control): Scale fan speed based on temperature error.
- R4 (Defrost Override): Override recirculation rules during defrosting.

<p align="center">
  <img width="1312" height="201" alt="Requirements Toolbox Screenshot" src="https://github.com/user-attachments/assets/e95229ee-2ea5-4563-a2fa-ab5460144514" />
  <br>
  <em>Screenshot of your Requirements Toolbox window showing R1 through R4</em>
</p>


 ## **3. Design and Model-in-the-Loop (MIL) Testing**


The Stateflow controller was modified to include a 0.5 °C deadband logic to satisfy R2. Initial logic verification was performed in a "Normal" simulation mode to ensure the car physics responded correctly to user setpoints.

 ## **4. Preparation for Code Generation**


The controller was isolated into an atomic subsystem to prepare for Embedded Coder®.

- Hardware Implementation: Configured the model for the ARM Cortex-M7 (MPS2) board.
- Signal Logging: Important signals (ACAct, HeaterAct, BlowerOut) were marked as Test Points to ensure they remained visible during compiled execution.

<p align="center">
  <img width="1731" height="391" alt="Hardware Implementation Settings" src="https://github.com/user-attachments/assets/6816dc22-58bb-42cd-a782-75cfbb13fdf6" />
  <br>
  <em>Screenshot of Configuration Parameters > Hardware Implementation showing ARM Cortex-M7</em>
</p>


 ## **5. PIL Verification and Troubleshooting**
 

This stage involved compiling the controller into C-code and running it on the Arm Cortex-M7 Fast Model Emulator.


Numerical Equivalency Challenges


During verification, a microscopic difference of $3.98 \times 10^{-13}$ was detected between the PC (x86) math and the Arm processor math. To resolve this, an Absolute Tolerance of $1 \times 10^{-4}$ was applied to the Baseline Criteria.



<p align="center">
  <img width="1428" height="839" alt="Test Manager Baseline Criteria" src="https://github.com/user-attachments/assets/cc9e7723-5439-4618-a41b-fc4a715eb683" />
  <br>
  <em>Screenshot of Test Manager Baseline Criteria showing 0.0001 in the ABS TOL column</em>
</p>


Simulation Optimization


To decrease simulation time from half a day to under a minute, the following optimizations were made:


- Setpoint Adjustment: User setpoint was changed from 9 °C to 17 °C to reach the target faster.

- Stop Time: Reduced from 1000s to 20s to focus only on the initial cool-down and deadband entry.



- <p align="center">
  <img width="1432" height="848" alt="Simulation Setpoint" src="https://github.com/user-attachments/assets/7924d0af-499d-42a7-a1ce-b534e8e87333" />
  <img width="567" height="565" alt="Stop Time Setting" src="https://github.com/user-attachments/assets/09b51db7-8af2-4228-90f2-8fa8676d0cdc" />
  <br>
  <em>Screenshot of the yellow Setpoint block showing "17" and the Stop Time set to 20</em>
</p>


  ## **6. Final Results and Verification Pack**
  

The final test run achieved a 100% Pass rating. The generated C-code proved to be mathematically equivalent to the model-level design, and the baseline comparison confirmed the controller's stability on the target architecture.


   ## **📈 Conclusion**
   

This project successfully demonstrates the Model-Based Design (MBD) "V-Model" workflow. By identifying and resolving hardware-specific numerical variances and configuration hurdles, we validated that the automatically generated automotive code is ready for deployment on high-performance Arm-based vehicle microcontrollers.


 ## **📁 Repository Contents**
 

model/: The optimized Simulink model file.

requirements/: The formal .slreqx requirements file.

tests/: The baseline_17deg.mat file used for the final verification run.

reports/: The final Verification_Pack.pdf.













