# CPU Monitoring and Auto-Scaling Application

## Overview
This project implements a system to monitor CPU usage on a local Virtual Machine (VM) and trigger auto-scaling to a cloud instance when CPU utilization exceeds a specified threshold. The system includes:
- A Bash script (`monitor_cpu.sh`) for CPU monitoring and auto-scaling.
- A Bash script (`start_stress.sh`) to simulate high CPU usage.
- A React-based frontend to visualize CPU utilization.
- Google Cloud integration for auto-scaling and workload migration.

## Prerequisites
Ensure the following dependencies are installed before running the application:
- Google Cloud SDK (`gcloud`)
- htop (for monitoring CPU usage)
- stress (for CPU load testing)
- Node.js and npm (for the frontend)

To install dependencies manually, run:
```sh
sudo apt update && sudo apt install -y htop stress curl jq git
```

## Setup Instructions

### 1. Clone the Repository
```sh
git clone https://github.com/your-repo/cpu-monitoring-app.git
cd cpu-monitoring-app
```

### 2. Authenticate Google Cloud
```sh
gcloud auth login
```
Verify that the correct project is set:
```sh
gcloud config list project
```

### 3. Run the Setup Scripts
Execute the setup sequence:
```sh
chmod +x dependencies.sh build.sh launch.sh gcp_permissions.sh stop.sh
./dependencies.sh
```
If the dependency installation is successful, proceed to build the application:
```sh
./build.sh
```
Once the build completes, launch the application:
```sh
./launch.sh
```
If an error occurs during the launch, run the following command to configure GCP permissions:
```sh
./gcp_permissions.sh
```
Retry launching the application:
```sh
./launch.sh
```

### 4. Stopping the Application
To stop all running processes, execute:
```sh
./stop.sh
```

## Application Workflow
1. `monitor_cpu.sh` continuously checks CPU usage.
2. If CPU usage exceeds the defined threshold, it:
   - Checks for an existing cloud instance.
   - Creates a new VM if required and migrates the workload via SCP.
   - Transfers computation tasks to the cloud.
3. `start_stress.sh` runs in the background to generate high CPU load for testing.
4. The React frontend provides a real-time CPU usage graph and control options.

## Accessing the Frontend
Once the application is running, the frontend can be accessed at:
```
http://localhost:3000
```

It includes:
- A real-time CPU usage graph.
- Controls to start/stop CPU-intensive tasks.
- A button to open the GCP console for monitoring cloud instances.

## Logs and Debugging
- CPU monitoring logs are stored in:
  ```sh
  ~/cpu-monitoring-app/monitor.log
  ```
- Stress test logs are stored in:
  ```sh
  ~/cpu-monitoring-app/stress.log
  ```
- To check running processes:
  ```sh
  ps aux | grep stress
  ```

## Troubleshooting
- If `gcloud auth login` fails, verify network connectivity.
- If the frontend does not start, check for missing dependencies and reinstall:
  ```sh
  cd frontend && npm install
  ```
- If the GCP VM does not launch, verify the project settings:
  ```sh
  gcloud config list
  ```

