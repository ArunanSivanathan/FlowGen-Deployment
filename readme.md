# Flow Extractor Setup Guide

This guide walks you through the setup of the **Flow Extractor** system using the `flowGen` (Flow Generator) and `flowReceiver` components.

> ⚠️ **Important Note:**  
> The `flowReceiver` provided here is for testing purposes only—to verify that `flowGen` can export flow data to an external server. A production-ready version with an embedded inference engine will be supplied once you confirm `flowGen` works correctly on the Banana Pi.

---

## 📦 Directory Structure

```
flowExtractor/
├── flowGen/
│   ├── flowGen
│   └── config.yaml
└── flowReceiver/
    ├── flowReceiver
    └── config.yaml
```

---

## 🔧 Setup Instructions

### 1. **Set up the Flow Receiver**

1. Download the `flowReceiver` binary and its `config.yaml` from the `flowReceiver/` directory.
    
2. Move both files to an **external server** (reachable from the Banana Pi):
    
    ```bash
    scp flowReceiver/config.yaml flowReceiver/flowReceiver <user>@<server_ip>:/home/<user>/flowReceiver/
    ```
    
3. SSH into the external server and verify that the receiver can be executed:
    
    ```bash
    ssh <user>@<server_ip>
    cd /home/<user>/flowReceiver/
    chmod +x flowReceiver
    ./flowReceiver
    ```
    

---

### 2. **Set up the Flow Generator on Banana Pi**

1. Download `flowGen` and its `config.yaml` from the `flowGen/` directory.
    
2. Get the MAC address of the `br-lan` interface:
    
    ```bash
    ifconfig
    ```
    
    Look for the `br-lan` section and copy the `HWaddr` (MAC address).
    ![ifconfig](https://github.com/ArunanSivanathan/FlowGen-Deployment/blob/main/images/ifconfig.png)
    
3. Open `config.yaml` and replace the `mac` field under `internalDevices` with the MAC address of the Banana Pi's `br-lan`.
    
4. In the same `config.yaml`, update the `MLServer` IP field to point to the external server running `flowReceiver`. Make sure it's reachable from the Banana Pi.
    
5. Move the modified files to the Banana Pi:
    
    ```bash
    scp flowGen/config.yaml flowGen/flowGen <user>@<banana_pi_ip>:/home/<user>/flowGen/
    ```
    
6. SSH into the Banana Pi and ensure executability:
    
    ```bash
    ssh <user>@<banana_pi_ip>
    cd /home/<user>/flowGen/
    chmod +x flowGen
    ```
    

---

## 🚀 Running the Components

> **Run `flowReceiver` first**, then launch `flowGen`.

On the **external server**:

```bash
cd /home/<user>/flowReceiver/
./flowReceiver
```

On the **Banana Pi**:

```bash
cd /home/<user>/flowGen/
./flowGen
```

---

## ⏱️ What to Expect

- Wait **approximately 60 seconds** after running `flowGen`.
    
- You should begin seeing incoming flow records on the `flowReceiver` side.
    

### Example Output of Flow Receiver:

![server output](https://github.com/ArunanSivanathan/FlowGen-Deployment/blob/main/images/serverOutput.png)
---

---

## 🧐 Next Steps

Once you verify that flow records are received correctly, you'll be provided with  **a server script** which includes the inference engine for real-time flow analysis.

---

## 📞 Troubleshooting

- If no flow is received after a minute:
    
    - Check if the Banana Pi can reach the server:  
        `ping <server_ip>`
        
    - Check the IP and MAC config in `config.yaml`.
        
    - Ensure both executables have the right permissions (`chmod +x`).
        

---

## 📍 Version

- flowGen: v0.0.1
    
- flowReceiver: v0.0.1 (test build)
