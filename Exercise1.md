# Exercise 1: Prerequisites Installation

In this exercise, you will set up your development environment with all necessary tools and authenticate to Azure. You'll use GitHub Copilot to help install and configure these tools.

> [!IMPORTANT]
> Make sure you have access to GitHub Copilot in your VS Code environment before starting this lab.

<details>
<summary><b>Step 1: Verify GitHub Copilot is Available</b></summary>

1. Login at https://github.com/enterprises/skillable-events/sso with Azure slice (GHE activated) credentials.
2. Once done, keep login page from SSO portal above open in browser.
3. In VS Code, press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac) to open the command palette.
4. Click on Copilot Icon in the bettom and then on the Sign in to use AI Features Option
   <img width="217" height="241" alt="image" src="https://github.com/user-attachments/assets/866cd0c9-de08-4aad-8859-08273bf0c4e4" />

5. Continue with GHE.com login option in VS Code
6  Authenticate profile through SSO portal 
7. Verify GitHub Copilot is active by looking for the Copilot icon in the bottom right status bar.
8. Open GitHub Copilot Chat by pressing `Ctrl+Alt+I` or clicking the chat icon in the sidebar.

> [!NOTE]
> If GitHub Copilot is not installed, use the Extensions marketplace to install "GitHub Copilot" and "GitHub Copilot Chat" extensions.

</details>

<details>
<summary><b>Step 2: Use GitHub Copilot to Install Azure CLI</b></summary>

1. Open GitHub Copilot Chat in VS Code (`Ctrl+Alt+I`).
2. Ask Copilot:

```
@terminal Check if Azure CLI is installed. If not, install it on Windows.
```

3. Review Copilot's suggestions and follow the installation steps.

**Alternatively, you can perform the following manually:**

```powershell
# Check if Azure CLI is already installed
if (Get-Command az -ErrorAction SilentlyContinue) {
    Write-Host "Azure CLI is already installed." -ForegroundColor Green
    az --version
} else {
    Write-Host "Azure CLI not found. Installing..." -ForegroundColor Yellow
    
    # Install Azure CLI using PowerShell
    $ProgressPreference = 'SilentlyContinue'
    Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi
    Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
    
    Write-Host "Installation complete." -ForegroundColor Green
}
```

4. After installation, verify by running:

```powershell
az --version
```

</details>

<details>
<summary><b>Step 3: Use GitHub Copilot to Install kubectl</b></summary>

1. In GitHub Copilot Chat, ask:

```
@terminal Check if kubectl is installed. If not, install it on Windows.
```

2. Follow Copilot's instructions.

**Alternatively, you can perform the following manually:**

```powershell
# Check if kubectl is already installed
if (Get-Command kubectl -ErrorAction SilentlyContinue) {
    Write-Host "kubectl is already installed." -ForegroundColor Green
    kubectl version --client
} else {
    Write-Host "kubectl not found. Installing..." -ForegroundColor Yellow
    
    # Download the latest kubectl release using Invoke-WebRequest
    $ProgressPreference = 'SilentlyContinue'
    Invoke-WebRequest -Uri "https://dl.k8s.io/release/v1.29.0/bin/windows/amd64/kubectl.exe" -OutFile "kubectl.exe"

    # Create a directory for kubectl
    New-Item -ItemType Directory -Force -Path C:\kubectl

    # Move kubectl to the directory
    Move-Item -Path kubectl.exe -Destination C:\kubectl\kubectl.exe -Force

    # Add to PATH (for current session)
    $env:Path += ";C:\kubectl"

    # Add to PATH permanently (requires admin)
    [Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\kubectl", [EnvironmentVariableTarget]::Machine)
    
    Write-Host "Installation complete." -ForegroundColor Green
}
```

3. Verify installation:

```powershell
kubectl version --client
```

Expected output:
```
Client Version: v1.29.0
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```

4. Test kubectl is working:

```powershell
kubectl cluster-info
```

> [!NOTE]
> If kubectl command is not recognized, close and reopen your terminal to refresh the PATH environment variable.

</details>

<details>
<summary><b>Step 4: Use GitHub Copilot to Install Git</b></summary>

1. Ask GitHub Copilot:

```
@terminal Install Git on my Windows machine
```

2. Follow the suggested commands:

```powershell
# Install Git using winget
winget install --id Git.Git -e --source winget
```

3. Verify installation:

```powershell
git --version
```

</details>

<details>
<summary><b>Step 5: Clone the Lab Repository using GitHub Copilot</b></summary>

1. In Copilot Chat, ask:

```
@terminal Clone the repository https://github.com/Trinanjan90/Test-and-deployment-automation-strategies-in-GHCP to my current directory
```

2. Copilot should suggest:

```bash
git clone https://github.com/Trinanjan90/Test-and-deployment-automation-strategies-in-GHCP.git
cd Test-and-deployment-automation-strategies-in-GHCP
```

3. Alternatively, if you have the local files, ask Copilot:

```
@workspace Open the folder containing the aks-store-quickstart.yaml file
```

</details>

<details>
<summary><b>Step 6: Authenticate to Azure using Azure CLI in terminal</b></summary>

1. Follow the below command:

```bash
az login --user "<username from resources tab> --password "<password from resources tab>"
```

3. After successful login, verify your subscription:

```bash
az account show
```

4. If you have multiple subscriptions, set the correct one:

```bash
az account set --subscription "<subscription-id>"
```

</details>

### Summary
✅ You have successfully set up your development environment with all required tools  
✅ Azure CLI, kubectl, and Git are installed  
✅ You are authenticated to Azure  
✅ GitHub Copilot is ready to assist you  
