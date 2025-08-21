# Simulation-of-bidirectional-power-flow-between-electric-vehicles-and-the-grid
## AIM
To Simulate bidirectional power flow between electric vehicles and the grid in MATLAB 

## APPARATUS REQUIRED
•	MATLAB

## MATLAB CODING
```
clc; clear; close all; 

total_time = 24;             
dt = 1/60;                   
time = 0:dt:total_time;      
battery_capacity_kWh = 60;   
V_batt = 400;                
SOC_initial = 0.5;           
SOC = SOC_initial;           
SOC_array = zeros(size(time)); 
P_ev = zeros(size(time));    

% SOC limits 
SOC_min = 0.2;               
SOC_max = 0.9;               

% Grid load demand profile (simulated) 
P_load = 3e3 + 2e3 * sin(2 * pi * time / 24);  % W 
% Thresholds to decide mode 
grid_demand_threshold = 4000;  % above this, enable V2G 
%% Simulation 
for i = 1:length(time) 
% Determine grid condition 
if P_load(i) > grid_demand_threshold && SOC > SOC_min 
% V2G mode (discharge EV to grid) 
P_ev(i) = -3000;  
elseif P_load(i) < grid_demand_threshold && SOC < SOC_max 
P_ev(i) = 3000;   
else 
P_ev(i) = 0;      
end 
energy_change_Wh = (P_ev(i) * dt);  % Power (W) × Time (hr) = Energy (Wh) 
delta_SOC = energy_change_Wh / (battery_capacity_kWh * 1000); 
SOC = SOC + delta_SOC; 
SOC = max(min(SOC, 1), 0); 
SOC_array(i) = SOC;   
end

%% Plot Results 
figure; 
subplot(3,1,1); 
plot(time, P_load/1000, 'k', 'LineWidth', 1.5); 
ylabel('Grid Load (kW)'); 
title('Grid Load Profile'); grid on; 
subplot(3,1,2); 
plot(time, P_ev/1000, 'b', 'LineWidth', 1.5); 
ylabel('EV Power (kW)'); 
title('EV Power Exchange (Positive = Charging, Negative = Discharging)'); 
grid on; 
subplot(3,1,3); 
plot(time, SOC_array * 100, 'r', 'LineWidth', 1.5); 
ylabel('SOC (%)'); xlabel('Time (hours)'); 
title('EV Battery State of Charge'); 
grid on; 
```

## Output
![WhatsApp Image 2025-08-21 at 11 27 31_8ad27697](https://github.com/user-attachments/assets/5b12119d-c641-4c4a-96dd-210d49a44a18)

## Result
    Thus, the bidirectional power flow between the electric vehicle and the grid was successfully simulated in the MATLAB.
