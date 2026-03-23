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


Right now, you verified the requirement by manually looking at a graph. In the automotive industry, you need this to be an automated pass/fail test so you can run it hundreds of times instantly. We will do this using Simulink Test.

Action Items:

Open Simulink Test: In your Simulink model window, go to the Apps tab at the top and click on Simulink Test.

Open the Test Manager: A new "Tests" tab will appear in your toolstrip. Click Simulink Test Manager (it usually has a clipboard icon).

Create a New Test File: In the Test Manager, click the New dropdown at the top left and select Test File. Save it in your project folder (e.g., Climate_MIL_Tests.mldatx).

Create a Baseline Test: Inside the new file, you will see a default test case.

Select the test case in the left pane.

In the right pane under System Under Test, click the "Use current model" button to link your climate control model.

Under the Baseline Criteria section, you can actually capture the exact successful run you just did! Click Capture to save the current outputs as your "Golden" baseline.

From now on, anytime you change the code or run it on the Arm processor, Simulink Test will automatically compare the new results to this golden baseline to make sure it still passes.

Step 4: Prepare the Controller for Code Generation

To ensure the controller is compatible with C code generation, we will use the Code Generation Advisor

Action Items:

Open Embedded Coder: Go back to your main sldemo_auto_climatecontrol Simulink window. Click on the Apps tab at the top and select Embedded Coder. A new C Code tab will appear in your toolstrip.

Select the Target Subsystem: Click once on your Stateflow Controller block so it is highlighted. This tells Simulink we are only focusing on this specific component.

Open the Advisor: In the C Code tab, look for the Check button (it might be a dropdown) and select Code Generation Advisor.

Choose Your Objectives: The Advisor will open in a new window. It will ask you to select your objectives (e.g., Execution efficiency, ROM efficiency, Traceability, or Safety Precaution). For this project, you can select Execution efficiency and click OK.

Run the Checks: In the left pane, you will see a folder tree of checks. Right-click the top folder (or click the Run Selected Checks button at the top). Simulink will now scan your controller logic to see if there are any unsupported blocks, weird data types, or inefficiencies that would make bad C code.

Review the Results: Look for any red "X" marks or yellow warning triangles.


<img width="1194" height="668" alt="image" src="https://github.com/user-attachments/assets/f87acec3-71c7-4796-9f44-59822563643e" />


<img width="1193" height="665" alt="image" src="https://github.com/user-attachments/assets/c053d879-fea5-434a-bb1d-026bc7d492f5" />


All checks passed and resolved step by step . 


Step 5: Generate C Code for the Controller Subsystem

Because we only want to generate code for the "brain" and leave the physical car environment as a simulation, we have to specifically command Simulink to build only that subsystem.

Action Items:

Select the Block: Go back to your main Simulink model diagram. Right-click directly on your Stateflow Controller block.

Build the Subsystem: In the right-click menu, navigate down to C/C++ Code and select Build This Subsystem.
**

Confirm the Build: A small window ("Build Subsystem") might pop up asking about tunable parameters. You don't need to change anything here—just click the Build button at the bottom of that window.

Wait for the Magic: MATLAB will now churn for a few moments. It is reading your logic, translating it to C code optimized for the Arm Cortex-M7, and compiling it. You can watch the progress in the Diagnostic Viewer at the bottom of your screen.

View the Report: Once it finishes, a Code Generation Report should automatically pop up in a new window.

Step 6: Run Processor-in-the-Loop (PIL) Tests












