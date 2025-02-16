# ADX IoT Analytics Demo
Azure Data Explorer can provide valuable insights into your Iot workloads. In the following Demo we look at thermostat IoT Devices that are in 3 different office buildings.

The following will deploy the following:
- IoT Central Store Analytics Template 
  - 36 thermostat devices being created and simulated
  - Setup Export to Event Hub of telemetry data
- Event Hub 
  - Data exported from IoT Central
  - ADX Data Connection to ingest data
- Azure Digital Twins
  - Office, Floors, and Thermostat twins
  - Atlanta, Dallas, Seattle offices with 6 Floors in each
  - 36 Thermostat twins created spread across the 3 offices with 2 on each floor
- Azure Data Explorer
  - StageIoTRaw table where data lands from Event Hub to get new data
  - Thermastat table with update policy to transform raw data
  - Historical data from January 2022 loaded into Thermostat table
  - Two functions
    - GetDevicesbyOffice: query ADT by Office names to get all DeviceIds at the office
    - GetDevicesbyOfficeFloor: query ADT by Office and Floor to get all Devices on that floor 

## Deployment instructions

On the [Azure Cloud Shell](https://shell.azure.com/) run the following commands to deploy the solution:
1. Login to Azure
    ```bash
    az login
    ```

2. If you have more than one subscription, select the appropriate one:
    ```bash
    az account set --subscription "<your-subscription>"
    ```

3. Get the latest version of the repository
    ```bash
    git clone https://github.com/bwatts64/ADXIoTAnalytics.git
    ```
    Optionally, You can update the patientmonitoring.parameters.json file to personalize your deployment.

4. Deploy solution
    ```bash
    cd PatientMonitoringDemo
    . ./deploy.sh
    ```

5. Finally, download the [Power BI report](/assets/Connected_Devices.pbix), update the data source to point to yoir newly deployed Azure Data Explorer database, and refresh the data in the report.

## Files used in the solution

- **asssets folder**: contains the following files:
  - AutomationPresentation.gif: quick explanation of the solution
  - Connected_Devices.pbix : sample report to visualize the data

- **config folder**: contains the configDB.kql that includes the code required to create the Azure Data Explorer tables and functions

- **dtconfig folder**: contains the files necessary to configure the Azure Digital Twins service:
  - Departments.json
  - Facility.json
  - KneeBrace.json
  - VirtualPatch.json

- **modules folder**: contains the [Azure Bicep](https://docs.microsoft.com/EN-US/azure/azure-resource-manager/bicep/) necessary to deploy and configure the resource resources used in the solution:
  - adx.bicep: ADX Bicep deployment file
  - digitaltwin.bicep: Digital Twin Bicep deployment file
  - eventhub.bicep: Event Hub Bicep deployment file
  - iotcentral.bicep: IoT Central Bicep deployment file
  - storage.bicep: Storage Bicep deployment file. This account is used as temporary storage to download ADX database configuration scripts)

- deploy.sh: script to deploy the solution. THe only one you need to run 
- main.bicep: main Bicep deployment file. It includes all the other Bicep deployment files (modules)
- patientmonitoring.parameters.json: parameters file used to customize the deployment
- README.md: This README file
