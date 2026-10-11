# Project Arachnid

Project Arachnid is a robotics and motion-control project focused on a quadruped / spider-style robot platform. It combines mechanical design, embedded control, sensor integration, SLAM, live dashboard telemetry, and a browser-based control interface.

This repository contains multiple related stacks and prototype code paths, but the active public-facing system is the `quadbot` dashboard and robot control stack.

## Project Goals

- Build a capable quadruped robot platform
- Provide a browser dashboard for drive, gait, and robot state monitoring
- Integrate LiDAR / mapping data for live navigation
- Support a camera stream for inspection and situational awareness
- Keep a clean public-facing expo interface while preserving advanced controls for debugging and calibration

## Repository Structure

- `quadbot/` — active robot control and dashboard stack
- `Controller/` — older controller and embedded stack
- `Data_Pipeline/` — laptop and pipeline bridge components
- `Bot_upper_structure_fusion/` — robot upper-body CAD / design files
- `hexapod-main New STL/` and `hexapod_spiderbot_model-main/` — mechanical models / STL references
- `Printing_Assembly/` — 3D-print assembly output
- `Servo_test/` — servo testing and PWM experiments
- `dashboard.html` — legacy standalone dashboard file

## Active Stack

The current active runtime stack is centered on:

- Python backend for robot control and websocket dashboard communication
- FastAPI + Uvicorn
- Web dashboard served from the project
- Robot configuration via `quadbot/config/robot.yaml`
- Lidar and avoidance logic
- Optional ROS2 / Foxglove integration for live mapping
- RealSense camera stream for browser playback

## Key Components

### Quadbot dashboard

The browser dashboard is the main runtime interface for the robot. It is designed to:

- show robot mode and health
- show driving controls
- show gait mode and motion state
- show map / SLAM data
- show camera feed
- expose advanced controls only when needed

Relevant files:

- `quadbot/quadbot/server.py`
- `quadbot/web/index.html`
- `quadbot/web/app.js`
- `quadbot/web/style.css`
- `quadbot/config/robot.yaml`

### Robot logic

The robot logic is split into modules for kinematics, gait, servo control, and hardware abstraction:

- `quadbot/quadbot/robot.py` — robot runtime and control loop
- `quadbot/quadbot/gait.py` — gait generation and movement logic
- `quadbot/quadbot/kinematics.py` — leg kinematics and coordinate transforms
- `quadbot/quadbot/servos.py` — servo limits, speed handling, calibration behaviors
- `quadbot/quadbot/hal.py` — hardware abstraction layer
- `quadbot/quadbot/lidar.py` — lidar handling and scan processing
- `quadbot/quadbot/avoid.py` — obstacle avoidance logic

### Legacy and reference code

The repository also contains older prototype or reference systems:

- `Controller/` — older robot control implementation
- `Data_Pipeline/` — laptop-side bridge and networking tools
- `dashboard.html` — earlier standalone interface prototype

These are useful for reference, but the active development path should stay on the `quadbot` system.

## Tech Stack

### Software

- Python 3
- FastAPI
- Uvicorn
- WebSockets
- JavaScript / HTML / CSS
- NumPy
- OpenCV / MJPEG-style camera streaming (for browser playback)
- Optional ROS2 + Foxglove for live SLAM visualization

### Hardware

- Raspberry Pi / laptop-based control host
- PCA9685 servo drivers
- LiDAR sensor
- RealSense D435 camera
- Servo actuators and quadruped chassis
- Battery / power management system

## Recommended Runtime Flow

For the public expo setup, use this pattern:

1. Run robot controller backend
2. Start camera stream
3. Start map / SLAM feed via Foxglove or equivalent
4. Serve the dashboard to the local browser
5. Show a single clean UI with:
   - live camera
   - live map
   - drive state
   - robot status
6. Keep expert calibration functions in an advanced or hidden section

## SLAM and Camera Integration

### Mapping

The preferred live mapping approach is:

- ROS2 topics feeding map data
- Foxglove Studio as the live map viewer
- the dashboard showing the map as part of the main demo interface

This is better than custom browser-side rendering because it is more reliable and better suited to live robot telemetry.

### Camera

The camera should be exposed as an actual image stream, not a raw HTML page embedded inside another page.

For a browser display, the stream should be served as a proper MJPEG or direct image endpoint so that the frontend can render it cleanly.

## Running the Project

### Setup

From the `quadbot` directory:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Start the dashboard

```bash
python -m quadbot.server
```

Then open the dashboard in a browser on the local network.

### Optional tests

```bash
pytest
```

## Expo Dashboard Design Guidance

The public dashboard should be simple and readable:

- map on one side
- camera on the other side
- drive and status controls in the center or lower area
- advanced calibration controls tucked away from the main view
- no cluttered control rail for every individual servo

This keeps the demo approachable for visitors while still preserving engineering access for calibration and debugging.

## Current Caveats and Notes

- The repository has multiple overlapping stacks; the active path is the `quadbot` folder.
- Some legacy code is still present and should not be treated as the primary runtime.
- Real hardware tuning is still required for servo ranges, gait tuning, and sensor placement.
- Map and camera availability depend on the actual ROS2 / camera runtime environment.
- A local network or hotspot is required for browser-based access.

## Recommended Next Steps

1. Validate the `quadbot` stack on the actual robot hardware
2. Confirm the RealSense stream route is working in the browser
3. Connect the Live SLAM data through Foxglove
4. Finalize the public dashboard layout for expo use
5. Hide advanced controls behind an expert section
6. Rehearse the full robot demo flow before the event

## Summary

Project Arachnid is a robotics development platform that combines motion control, navigation, live telemetry, and a browser dashboard. The proper public-facing approach is a simple one-screen dashboard with a live map and live camera, while advanced hardware tuning stays under an expert section.

This project is best approached as a practical robotics demo platform rather than a single monolithic codebase, and the most relevant active development path is within the `quadbot` directory.
