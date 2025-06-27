# azurevm
creating azure vm with integration in python
import subprocess
import json

def list_vm():
    print("Fetching VM list...")
    cmd = 'az vm list --output json'
    process = subprocess.run(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True, universal_newlines=True)

    if process.returncode == 0:
        vm_list = json.loads(process.stdout)
        for i, vm in enumerate(vm_list, start=1):
            name = vm['name']
            resource_group = vm['resourceGroup']
            print(f"{i}. {name} - {resource_group}")
    else:
        print(" Failed to fetch VM list.")

def create_group():
    list_vm()
    name = input("Enter the name of the resource group: ")
    location = input("Enter the location (e.g., eastus): ")
    command = f'az group create --name {name} --location {location}'
    process = subprocess.run(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True, universal_newlines=True)

    if process.returncode == 0:
        print(" Resource group created successfully.")
    else:
        print(" Failed to create resource group.")
        print(process.stderr)
def create_vm():
    list_vm()
    name = input("Enter the name of the VM: ")
    resource_group = input("Enter the resource group name: ")
    location = input("Enter the location: ")
    ssh_key = "/home/nishant/.ssh/id_rsa.pub"
    command = f"az vm create --resource-group {resource_group} --name {name} --image Ubuntu2204 --location {location} --admin-username azureuser --ssh-key-value {ssh_key} --size Standard_B1s"
    process = subprocess.run(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True, universal_newlines=True)

    if process.returncode == 0:
        print("VM created successfully.")
    else:
        print("Failed to create VM.")
        print(process.stderr)

def main():
    while True:
        print("\n===== Azure CLI Menu =====")
        print("1. List all VMs")
        print("2. Create a resource group")
        print("3. Create a virtual machine")
        print("4. Exit")
        choice=int(input("enter the choice"))
        if choice == 1:
            list_vm()
        elif choice == 2:
            create_group()
        elif choice == 3:
            create_vm()
        elif choice == 4:
            print("Goodbye!")
            break
        else:
            print("Invalid option. Try again.")

# Run the program
main()

                                                            
