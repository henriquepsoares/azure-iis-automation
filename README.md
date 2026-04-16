# 🚀 IIS Automation on Windows Server using Azure CLI

![Azure](https://img.shields.io/badge/Azure-Cloud-blue?logo=microsoftazure)
![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-blue?logo=windows)
![IIS](https://img.shields.io/badge/IIS-Web%20Server-green)
![VSCode](https://img.shields.io/badge/VSCode-Code-blue?logo=visualstudiocode)

## 💡 Overview

This project demonstrates how I fully automated the creation and configuration of an **IIS Web Server on Windows Server 2022**, using **Azure CLI**, with all scripts organized and stored in **Visual Studio Code** using the **`.azcli` file extension**.

The main goal was to build a **fast, repeatable, and fully automated solution**, from provisioning the virtual machine to publishing a custom web page on IIS.

---

## 🛠️ Technologies Used

- ☁️ **Microsoft Azure**
- 🖥️ **Windows Server 2022 Core**
- 🌐 **IIS (Internet Information Services)**
- ⚙️ **Azure CLI**
- 🧠 **PowerShell**
- 💻 **Visual Studio Code**

---

## 📂 Project Structure

- Scripts created and maintained in **VS Code**
- File extension used: **`.azcli`**
- Automation powered by **Azure CLI commands**
- Remote execution of **PowerShell commands** on the VM

---

## 🔐 Azure Login

```bash
az login
⚙️ Parameters Definition
rg=rg-vmwindows
location=brazilsouth
vm=vm-win2022
image=Win2022AzureEditionCore
adminUser=henrique



📦 Resource Group Creation
az group create -n $rg -l $location


🖥️ Virtual Machine Creation
az vm create \
  -g $rg \
  -n $vm \
  --image $image \
  --admin-username $adminUser \
  --admin-password Super202$


📋 VM Listing and Details
az vm list -g $rg
az vm list -g $rg -o yaml

az vm show -g $rg -n $vm
az vm show -g $rg -n $vm -o yaml
az vm show -g $rg -n $vm --query "name"


🌍 Get Public IP Address


az vm show -d -g $rg -n $vm --query publicIps -o tsv
🔓 Open HTTP Port (80)
az vm open-port --port 80 -g $rg --name $vm


🌐 Connectivity Test
curl <PUBLIC_IP>

🧩 Install IIS Web Server (Remote PowerShell)

az vm run-command invoke \
  -g $rg \
  -n $vm \
  --command-id RunPowerShellScript \
  --scripts "Install-WindowsFeature -Name Web-Server -IncludeManagementTools"

🎨 Deploy Custom Web Page

az vm run-command invoke \
  -g $rg \
  -n $vm \
  --command-id RunPowerShellScript \
  --scripts 'Set-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value "Hello from Azure Virtual Machine!"'

🧹 Environment Cleanup (Optional)

az group delete -n $rg -y

🎯 Final Result

✅ Automated VM provisioning
✅ IIS installation without manual access
✅ Custom web page deployed via script
✅ Infrastructure as Code (IaC) approach
✅ Fast, scalable, and reusable process

⭐ Conclusion

This project highlights how to automate infrastructure on Azure using professional cloud and DevOps practices, combining Azure CLI, PowerShell, and version control.