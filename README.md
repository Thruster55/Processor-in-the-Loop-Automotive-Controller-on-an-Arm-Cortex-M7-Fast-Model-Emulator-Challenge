# Processor-in-the-Loop-Automotive-Controller-on-an-Arm-Cortex-M7-Fast-Model-Emulator-Challenge

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MATLAB Compatibility](https://img.shields.io/badge/MATLAB-R2025b-blue.svg)](https://www.mathworks.com/products/matlab.html)
[![Status](https://img.shields.io/badge/Status-Ongoing-success.svg)]()

Since its a new concept for me , I started with introducing myself with what the project is . So I saw this video for my introduction to Processor in loop simulations - https://www.youtube.com/watch?v=y6pwmtJWTl8 and also this article - https://de.mathworks.com/help/ecoder/armcortexm/ref/getting-started-with-embedded-coder-support-package-for-arm-cortex-m-processors.html .


Step 1: Select Your Application and Explore the Example
- Selected application - Automatic Climate Control

Here we will define the high level requirements 
For that we have to add on - Requirements Toolbox 

The primary requirement - The system shall maintain the cabin temperature within ±1 °C of the setpoint under all operating conditions
. Now linkking the requirement to the state flow chart

Step 2: List of excellent high-level requirements
-The Deadband Requirement - Prevent Rapid Cycling  (The system shall turn off both the active heater and the AC compressor when the cabin temperature is within ±0.5 °C of the user's setpoint to prevent the system from rapidly turning on and off)
-The Blower Fan Requirement - Proportional Blower Control (The system shall adjust the blower fan speed proportionally to the temperature differential. The fan shall power down completely when the cabin temperature is within 0.5 °C of the setpoint)
-The Defrost Safety Interlock - Defrost Air Recirculation Override (When the driver activates the windshield Defrost mode, the system shall automatically disable air recirculation to ensure fresh, dry air is used to clear the windshield)

Step 3: Modify or Redesign the Controller to meet your requirements
But before we change any logic, we must perform a "baseline test" to see how the default controller actually behaves against the requirements we just wrote

Here how to execute this test using Simulink's built-in data logging 
We first run baseline test 
- Mark Your Signals for Logging
  These are the two signals we set for logging User Setpoint & Cabin Temperature
-Run the Simulation
- Inspect the Data  - Now inspecting the data inspector and looking whether the actual cabin temperature is tracing the setpoint temperature also is the ±1 °C rule violated or not .

  Now affter inspecting baseline graph as shown here you can get the idea that the user setpoint is a flat line at 282 . The cabin starts hot (around 291 K / 18 °C), and the controller kicks in, bringing the temperature down rapidly and by the 100-second mark, the cabin temperature settles at exactly 282.5 and stays there perfectly flat . The difference between your setpoint (282.0) and actual temperature (282.5) is 0.5 degrees , it passes the baseline test. 

  <img width="1086" height="826" alt="image" src="https://github.com/user-attachments/assets/64f98732-ca34-4537-9148-5611817ae6d5" />


  Now next step

  Step 4: Design and Build Model-in-the-Loop (MIL) Test Cases

For that we will need Simulink Test Toolbox 






















