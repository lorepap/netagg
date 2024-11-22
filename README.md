# Dynamic In-Network Aggregation Framework Benchmark

This repository showcases the performance and capabilities of a dynamic in-network aggregation framework using a programmable switch, along with physical workers and a parameter server (PS), based on ATP. The setup includes two physical workers, one physical PS, and a programmable switch.

---

## Getting Started

To get started, clone the repository:

```bash
git clone https://github.com/lorepap/netagg.git
```

### Setup Instructions

#### Step 1: Compile P4 Program and Start Tofino Model (Terminal 1)

If you are using a physical switch, compile the switch program and proceed to Step 2 directly.

```bash
# Navigate to the SDE directory
cd $SDE
```

```bash
# Compile the P4 program
$TOOLS/p4_build.sh ~/git/p4ml/p4src/p4ml.p4
```

```bash
# (Optional) Start the software Tofino behavior model
./run_tofino_model.sh -p p4ml
```

#### Step 2: Load the Switch Program (Terminal 2)

```bash
# Navigate to the SDE directory
cd $SDE
```

```bash
# Run the switch daemon with the specified P4 program
./run_switchd.sh -p p4ml
```

#### Step 3: Enable Ports and Install Table Entries (Terminal 3)

```bash
# Run P4 test scripts
$SDE/run_p4_tests.sh -t $ATP_REPO/ptf/ -p p4ml
```

```bash
# Install entries via the RPC script
$TOOLS/run_pd_rpc.py -p p4ml $ATP_REPO/run_pd_rpc/setup.py
```

### Running the Parameter Server

#### Step 4: Compile and Start the Server (Terminal 4)

```bash
# Navigate to the server directory
cd $ATP_REPO/server/
```

```bash
# Compile the server application
make
```

```bash
# Run the server application
# Usage: ./app [AppID]
sudo ./app 1
```

Wait until all threads have completed QP creation.

### Running the Workers

#### Step 5: Compile the Worker Application

```bash
# Navigate to the client directory
cd $ATP_REPO/client/
```

```bash
# Compile the worker application
make
```

#### Step 6: Run Worker 1 (Terminal 5)

```bash
# Run Worker 1
# Usage: ./app [MyID] [Num of Workers] [AppID] [Num of PS]
sudo ./app 0 2 1 1
```

#### Step 7: Run Worker 2 (Terminal 6)

```bash
# Run Worker 2
# Usage: ./app [MyID] [Num of Workers] [AppID] [Num of PS]
sudo ./app 1 2 1 1
```

Once the workers are running, switch back to Terminals 5 or 6 to monitor the bandwidth report in real-time.

---

Feel free to modify the configuration or parameters as needed to suit your experimental setup.
