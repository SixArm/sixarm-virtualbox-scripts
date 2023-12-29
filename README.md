# SixArm VirtualBox Scripts

## Example commands

List vms:

```sh
vboxmanage list vms
```

Delete a vm:

```sh
vboxmanage unregistervm <uuid> --delete
```

Remove a vm that is inaccessible:

```sh
vboxmanage unregistervm <uuid>

List drives:

```sh
vboxmanage list hdds
```

Delete a drive:

```sh
vboxmanage closemedium <uuid> --delete
```

List operating system types:

```sh
vboxmanage list ostypes
```

## Formats for disks

We typically use these formats:

* VMDK: VMWare uses VMDK as the default disk image format.
  Multipe VMDK versions and variations exist, so it’s important
  to understand which one you’re using and where it can be used.

* VDI: VirtualBox uses VDI as the default disk image format.
  VDI is not compatible with VMWare.


## Encryption

Question: Are there performance and/or security advantages to using 
the VirtualBox Disk Encryption over oraclelinux disk encryption in the VM?

Answer: As I understand it, a good point of the VirtualBox encryption is 
you can easily change your mind, encrypt a VM that isn't or decrypt a VM
which is, and use the result with VirtualBox. Making a decrypted image 
from an oraclelinux LUKS-encrypted one and vice-versa is likely possible but 
would be more complicated.

Also, with the VirtualBox encryption you can store the encryption 
passphrase in the VB config outside of the VM, so you can boot the VM
without having to enter a decryption passphrase. Of course you have to
keep the VB config safe to avoid disclosure of the passphrase.

Another benefit of using VirtualBox encryption is that you can securely
save the VM state to resume later. Encrypted VMs have their state file 
encrypted as well. In contrast, if you use oraclelinux full-disk encryption 
features of the guest, then saving the VM state will effectively leak 
the encryption key to the host's storage in the state file.

See https://superuser.com/questions/1445735/virtualbox-disk-encryption-vs-oraclelinux-vm-disk-encryption


## How to encrypt

To support VirtualBox Disk Encryption of the virtual machine, you need 
to install VirtualBox Extension Pack, available at the VirtualBox site.

The Extension Pack is not included by default, because it can contain 
system level software that could be potentially harmful to your system.

The version of Extension Pack needs to match your VirtualBox version.
So in case of installation issues, try to shut down all your VMs, then
then upgrade your VirtualBox.

After you install Extension Pack, the encrypt operation can use the
command-line interface with this syntax:

```sh
VBoxManage encryptmedium "uuid|filename" \
--newpassword "file|-" \
--cipher "cipher id" \
--newpasswordid "id"
```

See https://superuser.com/questions/1072752/how-to-encrypt-vm-box-via-vboxmanage


## More information

https://spin.atomicobject.com/2013/06/03/ovf-virtual-machine/

https://github.com/jedi4ever/veewee
