# Testing Nextflow on Azure

## Setting up Azure to run nextflow

Create a resource group `RNAseq` in the location `East US`.

`az group create --location eastus --name RNAseq`

Create a Ubuntu LTS virtual machine in the `RNAseq` resource group and generate a pair os sss of keys.

`az vm create -n RNAseqVM -g RNAseq --image UbuntuLTS --generate-ssh-keys`

Deploy nextflow using the nextflow-azure deployment template (indicated here as azuredeploy.json)

`az deployment group create --name rnaseq --resource-group RNAseq --template-file azuredeploy.json`

## Running the RNAseq Nextflow pipeline on Azure
