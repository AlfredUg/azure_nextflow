# Testing RNAseq Nextflow pipeline on Azure

## Setting up Azure to run nextflow

Create a resource group `RNAseq` in the location `East US`.

`az group create --location eastus --name RNAseq`

```
{
  "id": "/subscriptions/e5638e24-f911-490e-bed6-50993a9e0d36/resourceGroups/RNAseq",
  "location": "eastus",
  "managedBy": null,
  "name": "RNAseq",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

Create a Ubuntu LTS virtual machine in the `RNAseq` resource group and generate a pair of ssh keys.

`az vm create -n RNAseqVM -g RNAseq --image UbuntuLTS --generate-ssh-keys`

```
SH key files '/home/alfred/.ssh/id_rsa' and '/home/alfred/.ssh/id_rsa.pub' have been generated under ~/.ssh to allow SSH access to the VM
. If using machines without permanent storage, back up your keys to a safe location.
{- Finished ..
  "fqdns": "",
  "id": "/subscriptions/e5638e24-f911-490e-bed6-50993a9e0d36/resourceGroups/RNAseq/providers/Microsoft.Compute/virtualMachines/RNAseqVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-A6-D1-BF",
  "powerState": "VM running",
  "privateIpAddress": "XX.X.X.X",
  "publicIpAddress": "XX.XXX.XXX.XX",
  "resourceGroup": "RNAseq",
  "zones": ""
}
```

Log into the VM created above to make sure it worked fine.

```
ssh -i ~/.ssh/id_rsa alfred@XX.XXX.XXX.XX

The authenticity of host 'XX.XXX.XXX.XX (XX.XXX.XXX.XX)' can't be established.
ECDSA key fingerprint is SHA256:YTbuc27gRAvRmC6O7yhPJ83Sgft3rKrTxJ5oazJktpY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'XX.XXX.XXX.XX' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.3.0-1028-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Jun 26 10:52:32 UTC 2020

  System load:  0.13              Processes:           108
  Usage of /:   4.3% of 28.90GB   Users logged in:     0
  Memory usage: 7%                IP address for eth0: XX.X.X.X
  Swap usage:   0%

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

Before deployment, the template requires that we supply the DNS label, admin username and admin password or key. So we configure the DNS of the VM created above on azure portal ahead of deployment. In this case, we set the DNS label to 'rnaseqazure'.

![alt text](https://github.com/AlfredUg/azure_nextflow/blob/master/DNS_Configuration.png)


Deploy nextflow using the nextflow-azure deployment template; <https://github.com/grbot/azure-quickstart-templates/blob/master/nextflow-genomics-cluster-ubuntu/azuredeploy.json>

`az deployment group create --name rnaseq --resource-group RNAseq --template-file azuredeploy.json`

Currently, the deployment runs and fails halfway with a quota exceeded error and this what we trying to resolve now.
