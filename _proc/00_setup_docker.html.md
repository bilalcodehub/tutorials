---
output-file: setup_docker.html
title: Installing Docker for Machine Learning in Medicine

---



<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

### Step 1: Install Docker

**Note:** Docker Compose is now bundled with Docker, so a separate installation of `docker-compose` is no longer necessary. We will walk through platform-specific installation instructions for Docker.

### Docker Installation on Linux

1. **Update Your Packages**:  
   Before installing Docker, update your package repository:
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

2. **Install Required Packages**:  
   Install dependencies for Docker installation:
   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Add Docker’s Official GPG Key and Repository**:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   sudo add-apt-repository "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

4. **Install Docker**:  
   After adding Docker’s official repository, install Docker:
   ```bash
   sudo apt update
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

5. **Verify Installation**:  
   Check if Docker is installed correctly by running:
   ```bash
   docker --version
   ```

6. **Add Your User to the Docker Group (Optional)**:  
   To run Docker commands without `sudo`:
   ```bash
   sudo usermod -aG docker $USER
   ```
   Log out and back in again for this to take effect.

7. **Test the Installation**:  
   Run a test container to ensure Docker is working correctly:
   ```bash
   docker run hello-world
   ```

### Docker Installation on Windows

1. **Install Docker Desktop**:  
   Docker Desktop is required for Windows. You can download it from [Docker’s official page](https://www.docker.com/products/docker-desktop).

2. **Enable WSL2 (Windows Subsystem for Linux)**:  
   Docker Desktop for Windows requires WSL2. To install and configure WSL2:
   - Open PowerShell as an Administrator and run:
     ```bash
     wsl --install
     ```
   - Reboot your system if prompted.

3. **Install Docker Desktop**:  
   - Run the Docker Desktop installer and follow the installation prompts.
   - After installation, start Docker Desktop from the Start Menu.

4. **Verify Installation**:  
   Open a terminal (PowerShell or CMD) and run:
   ```bash
   docker --version
   ```

5. **Test the Installation**:  
   To test Docker, run the following in your terminal:
   ```bash
   docker run hello-world
   ```

### Docker Installation on macOS

1. **Download Docker Desktop for Mac**:  
   You can download the Docker Desktop installer for macOS from [Docker’s official page](https://www.docker.com/products/docker-desktop).

2. **Install Docker Desktop**:  
   - Double-click the downloaded `.dmg` file and drag the Docker icon to your Applications folder.
   - Open Docker from your Applications.

3. **Verify Installation**:  
   Once Docker is installed, open a terminal and check the version:
   ```bash
   docker --version
   ```

4. **Test the Installation**:  
   Run the following command to check if Docker is working correctly:
   ```bash
   docker run hello-world
   ```

### Common Issues and Troubleshooting

Here are some common issues users might face during Docker installation:

1. **Permission Denied (Linux)**:  
   If you get a permission error when running Docker commands without `sudo`, ensure that your user is added to the `docker` group as explained above:
   ```bash
   sudo usermod -aG docker $USER
   ```

2. **WSL2 Errors (Windows)**:  
   If Docker Desktop fails to run on Windows, ensure that WSL2 is properly installed. You can manually update your WSL kernel with:
   ```bash
   wsl --update
   ```

3. **macOS Network Issues**:  
   If Docker cannot access the internet, ensure that it is not blocked by your firewall or network settings. Restarting Docker Desktop can also resolve these issues.

### Next Steps

Congratulations! Docker is now installed and running on your system. In the next notebook, we will cover how to set up MONAILabel and CVAT using Docker. Ensure Docker is running in the background as it will be required in subsequent tutorials.

