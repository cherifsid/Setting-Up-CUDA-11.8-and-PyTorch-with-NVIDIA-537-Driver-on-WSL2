# Setting Up CUDA 11.8 and PyTorch with NVIDIA 537 Driver on WSL2

This tutorial helps you install **WSL2**, **NVIDIA 537 driver**, **CUDA 11.8**, and **PyTorch** with GPU support on WSL2. The environment will be set up for GPU-accelerated machine learning using PyTorch.

### Prerequisites:
- Windows 10/11
- NVIDIA GPU supported by CUDA 11.8
- Administrator privileges

### Step 1: Install WSL2

1. Open PowerShell as Administrator and run:

    ```bash
    wsl --install
    ```

    Restart your system after the installation completes.

2. After reboot, install a Linux distribution (e.g., **Ubuntu**) from the Microsoft Store.

3. If not set to default, configure WSL2 as the default version:

    ```bash
    wsl --set-default-version 2
    ```

4. Update the WSL kernel (optional but recommended):

    ```bash
    wsl --update
    ```

5. Launch your Linux distribution (e.g., **Ubuntu**) and update the package list:

    ```bash
    sudo apt update && sudo apt upgrade
    ```

---

### Step 2: Install NVIDIA Driver 537

1. Download and install the **NVIDIA 537 driver** from the [NVIDIA website](https://www.nvidia.com/Download/driverResults.aspx/212729/en-us).
2. After installation, reboot your system.
3. Verify driver installation by running:

    ```bash
    nvidia-smi
    ```

    Ensure the **NVIDIA Driver Version** is 537, and CUDA version matches 11.8.

---

### Step 3: Install CUDA 11.8 on WSL2

1. Download and install the CUDA toolkit:

    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
    sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
    wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
    sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
    sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
    sudo apt-get update
    sudo apt-get -y install cuda
    ```

2. Set up environment variables by adding these lines to your `~/.bashrc` file:

    ```bash
    export PATH=/usr/local/cuda/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
    ```

3. Apply the changes:

    ```bash
    source ~/.bashrc
    ```

4. Verify CUDA installation:

    ```bash
    nvcc --version
    ```

---

### Step 4: Install PyTorch with CUDA 11.8 Support

1. Create a Python virtual environment using **Miniconda** or **venv**:

    ```bash
    conda create --name llm-gpu-env python=3.10
    conda activate llm-gpu-env
    ```

2. Install PyTorch with CUDA 11.8 using `pip`:

    ```bash
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```

---

### Step 5: Verify PyTorch GPU Setup

1. Open a Python shell and verify GPU availability with PyTorch:

    ```python
    import torch
    print("CUDA is available:", torch.cuda.is_available())
    print("Number of GPU devices:", torch.cuda.device_count())
    print("Device name:", torch.cuda.get_device_name(0))
    ```

---

### Troubleshooting
- Ensure you have the correct NVIDIA driver (537).
- Check that your `~/.bashrc` file has the correct environment variables for CUDA.
