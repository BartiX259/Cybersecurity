# Hypervisor Internals

## Type 1 - Bare Metal

Hypervisor has direct access to hardware, no operating system in between.
Great performance and scalability, used to run many VMs.
Examples: HyperV, VMware ESXi


## Type 2 - Hosted

Runs on top of an os. Usually found in dev/user machines.
Great ease of use, free offerings.
Examples: VirtualBox, VMware Workstation

## Hypervisors in security

VMs are great for analyzing malware, replicating real world networks and pen testing.
Hypervisors are a big target for attackers, as getting access to one grants access to the VMs running on it.
Microsoft offers big bounties for discovering vulnerabilities in Hyper-V.

## Internals

- vCPU - mapped to real CPU cores, but not permanently assigned to one. Hypervisor spreads vCPU instructions across physical cores as needed.
- Virtual storage - illusion of a hard drive, VMDK/VDI.
- vNIC - virtual network interface:
    - bridged - gets ip from host's network
    - NAT - network activity appears as the host
    - host-only - only accessible from the host
    - specific - manual ip and subnet

Paravirtualisation - the VM is aware that it's using virtual hardware. Better performance but the
VM os has to support it. Generally used for disk and network IO but not CPU/RAM.

Nested Virtualisation - run VM in VM. Hardware virtualisation is needed (Intel VT-x/AMD-V).

## Guest additions

- Better performance/graphics
- Shared folders
- Shared clipboard
- USB devices

To use, mount the guest addition ISO in the VM's CD tray.

Might introduce vulnerabilities, as they have to run with increased priviledges.
