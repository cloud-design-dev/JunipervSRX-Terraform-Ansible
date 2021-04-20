# Overview
This guide will show you how to use Terraform and Ansible to deploy and configure a [Juniper vSRX]() network gateway on the IBM Cloud. 

## Deploy all resources

1. Copy `terraform.tfvars.example` to `terraform.tfvars`:
   ```sh
   cp terraform.tfvars.example terraform.tfvars
   ```
1. Edit `terraform.tfvars` to match your environment.

   | Name | Description | Required |
   | ---- | ----------- | ---|
   | iaas_classic_username | IBM Cloud Classic Username | Y |
   | iaas_classic_api_key | IBM Cloud Classic User API Key | Y |
   | datacenter | The datacenter where the vSRX will be deployed | Y |
   | network_speed | The networking speed for the vSRX interfaces | Y |
   | ssh_key | Name of an existing SSH key to inject in to the vSRX | N |
   | hostname | Hostname for the vSRX Cluster | N | 
   | domain | Domain name for the vSRX Cluster | N | 

1. Plan deployment:
   ```sh
   terraform init
   terraform plan -out default.tfplan
   ```
1. Apply deployment:
   ```sh
   terraform apply default.tfplan
   ```

> Note: It is possible that the `package_key_name`, `process_key_name`, or `os_key_name` could change as new versions of the gateway appliance are released. If you receive an error related to any of these options, the error message will tell you the currently available options. Update the code and re-run your plan / apply to pick up the changes. 

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| iaas\_classic\_username | The IBM Cloud Classic Infrastructure Username. | `string` | n/a | yes |
| iaas\_classic\_api\_key | The IBM Cloud Classic Infrastructure API key. | `string` | n/a | yes |
| datacenter | The datacenter where the vSRX Gatewally Appliance is deployed. | `string` | n/a | yes |
| hostname | Name of the vSRX Gateway Appliance. | `string` | n/a | yes |
| network\_speed | Default networking interface speed for the vSRX. | `string` | `1000` | yes |
| ssh\_key\_ids | List of SSH key IDs to inject into vSRX host | `list(string)` | n/a | no |
| tags | List of tags to add on all created resources | `list(string)` | `[]` | no |
| private\_network\_only | description | `bool` | `false` | no |
| tcp\_monitoring | description | `bool` | `false` | no |
| redundant\_network | description | `bool` | `false` | no |

## Outputs
| Name | Description | 
|------|-------------|
| id | The unique identifier of the network gateway |
| public\_ipv4\_address | The public IP address of the network gateway |
| private_ipv4_address | The private IP address ID of the network gateway |
| public\_vlan\_id | The public VLAN ID for the network gateway. |
| private\_vlan\_id | The private VLAN ID of the network gateway. |
| associated\_vlans | A nested block describing the associated VLANs for the member of the network gateway |