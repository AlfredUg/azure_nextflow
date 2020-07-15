# Testing RNAseq Nextflow pipeline on Azure

## Setting up Azure to run nextflow

Create a resource group `RNAseq` in the location `East US`.

`az group create --location eastus --name RNAseq`

Create a Ubuntu LTS virtual machine in the `RNAseq` resource group and generate a pair of ssh of keys.

`az vm create -n RNAseqVM -g RNAseq --image UbuntuLTS --generate-ssh-keys`

Deploy nextflow using the nextflow-azure deployment template; <https://github.com/grbot/azure-quickstart-templates/blob/master/nextflow-genomics-cluster-ubuntu/azuredeploy.json>

`az deployment group create --name rnaseq --resource-group RNAseq --template-file azuredeploy.json`

## Running the RNAseq Nextflow pipeline on Azure
