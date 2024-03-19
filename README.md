# AWS Windows EC2 Driver Upgrade

This repository contains scripts and documentation for upgrading AWS drivers on Windows EC2 instances. The provided PowerShell scripts facilitate the listing of current AWS drivers, and the installation or upgrade of the Elastic Network Adapter (ENA) driver, as well as AWS NVMe drivers.

## Getting Started

These instructions will guide you through the process of using the scripts to upgrade AWS drivers on your Windows EC2 instances.

### Prerequisites

Ensure that you have administrative access to your Windows EC2 instance to execute these scripts.

**Important Notes Before Updating Drivers:**

1. **Take Downtime:** Schedule a maintenance window or take downtime before updating drivers to minimize the impact on your services and users.

2. **Backup with AMI:** Create an Amazon Machine Image (AMI) of your EC2 instance before updating the drivers. This provides a recoverable snapshot of your instance, allowing for a quick restoration if needed.

### Listing AWS Drivers

To list the AWS and Amazon drivers currently installed on your Windows EC2 instance, execute the following PowerShell command:

```powershell
Get-WmiObject Win32_PnpSignedDriver | Select-Object DeviceName, DriverVersion, InfName | Where-Object {$_.DeviceName -like "*AWS*" -OR $_.DeviceName -like "*Amazon*"}
```

### Installing or Upgrading the ENA Driver

1. Open PowerShell as an administrator.
2. Enable TLS 1.2:

   ```powershell
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   ```

3. Download the latest ENA driver:

   ```powershell
   Invoke-WebRequest https://ec2-windows-drivers-downloads.s3.amazonaws.com/ENA/Latest/AwsEnaNetworkDriver.zip -Outfile $env:USERPROFILE\AwsEnaNetworkDriver.zip
   ```

4. Extract and install the driver:

   ```powershell
   Expand-Archive $env:USERPROFILE\AwsEnaNetworkDriver.zip -DestinationPath $env:USERPROFILE\AwsEnaNetworkDriver
   cd $env:USERPROFILE\AwsEnaNetworkDriver
   .\install.ps1
   ```

### Installing or Upgrading AWS NVMe Drivers

1. Enable TLS 1.2:

   ```powershell
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   ```

2. Download the latest AWS NVMe drivers:

   ```powershell
   Invoke-WebRequest https://s3.amazonaws.com/ec2-windows-drivers-downloads/NVMe/Latest/AWSNVMe.zip -Outfile $env:USERPROFILE\nvme_driver.zip
   ```

3. Extract and install the driver:

   ```powershell
   Expand-Archive $env:USERPROFILE\nvme_driver.zip -DestinationPath $env:USERPROFILE\nvme_driver
   cd $env:USERPROFILE\nvme_driver
   .\install.ps1
   ```
## License

This project is licensed under the MIT License - see the LICENSE.md file for details.
