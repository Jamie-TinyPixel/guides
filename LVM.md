## Checking free space: 

```javascript
lsblk # list disk and filesystems
vgs # check free space
parted /dev/sde unit GB print free | grep GB           # check free space on raw drive
parted /dev/sda unit GB print free | grep 'Free Space' | tail - n1 | awk '{print $3}'
```

## Formatting new drive (NEED TO REWRITE THIS ONE - IGNORE)

```javascript
fdisk /dev/sdb
n
p
1
t
8e
w
```

## Creating new pv volume

```javascript
pvcreate /dev/sdb1
# pvs
  PV              VG               Fmt   Attr      PSize      PFree
  /dev/sda2  VolGroup00 lvm2 a--u    49.72g     32.84g
  /dev/sdb1  VolGroup00 lvm2 a--u    249.97g    0
```

## Creating new volume group 

```javascript
vgcreate docker-vg /dev/sde1 /dev/sdf1 /dev/sdg1
```

## Extending existing volume group 

```javascript
vgextend docker-vg /dev/sdb1
```

## Creating new Logical Volume

```javascript
lvcreate -n docker-vg-sn -L 250G VolGroup00 # standard way
lvcreate -n docker-vg-myapp VolGroup01 -l +100%free /dev/sde1 # this can be useful also
```

## Listing Logical Volume

```javascript
root@docker:~# lvs
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  docker    docker-vg -wi-ao----  30.00g
  ubuntu-lv ubuntu-vg -wi-ao---- <24.00g
root@docker:~# lvs -o +devices # this can be useful also
```

## Adding new file system 

Or manually:

```javascript
mkfs.ext4 /dev/mapper/docker--vg-volumes
mkdir /opt/docker/volumes/
echo '/dev/mapper/docker--vg-docker--vg--volumes  /opt/docker/volumes/   ext4        defaults,nodev   1       2' >> /etc/fstab
mount /opt/docker/volumes/
df -h
```

Or with Ansible:

```javascript
cd /apps/ansible
ansible-playbook /apps/ansible/playbooks/rhel/localuser/localuser.yml -i /home/sca37/ansible_host_files/localuser_hosts -e target=PEOPLEDAT -e groupname=psoft -e username=psoftadm
```

## 

# LVS extend:

## extend underlayer root lvm volume in 1Gb

```javascript
lvextend -L +750M /dev/mapper/VolGroup00-apps_apache -r                                                  # -r option will refresh it after that that will add 750M more to existing volume (so for example 1G +750M = 1.75G)
lvextend -L 2G /dev/mapper/VolGroup00-apps_apache -r                                                     # that will setup new volume size to exactly 2 GB
lvextend -l +100%free /dev/mapper/VolGroup00-cmmapdbprd--ora--archivelog01 /dev/sdc1                     # this can be useful too
```

## online resize the apache filesystem

```javascript
resize2fs /dev/mapper/VolGroup00-apps_apache
```

## confirm the new size is 1G

```javascript
df -h /apps/apache/
```

## LVS manual remove:

```javascript
umount /apss/cooker
vi /etc/fstab # then remove automount
lvremove apps_cooker
```

# Back out

## Detail the steps to backout the change.

> **Contact with application owner and ask for stop it.**
> 
> ```
> umount /apps/ora
> lvdisplay dev/mapper/VolGroup00-apps_ora
> lvreduce -L 6G /dev/mapper/VolGroup00-apps_ora
> resize2fs /dev/mapper/VolGroup00-apps_ora
> mount /apps/ora
> df -h /apps/ora
> ```

### How long will it take (ensuring the timings for the change include the backout)?

> Around 5 min
> At what point will a decision be made to backout if required?
> If file system cannot be resized. DO NOT REDUCE correctly resized file system without umount!

## Have you tested your backout and what was the outcome, if not why are you unable to test the backout?

> This is standard unix task and don't  need testing. 

## Risk Assessment

> What services will be impacted during the implementation and how will they be impacted?
> It's full online change - no service impact.
> If the change went wrong what services are at risk and how could they be affected?
> Very unlikely worst case scenario:
> Integration Hub lost file system.
> What is the chance of these services being affected and why?
> Very unlikely

# Another useful comand:

Rescan device:

```javascript
/bin/echo 1 > /sys/block/sda/device/rescan
```

List all devices:

```javascript
root@ud003671 # lsblk
NAME                                         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
fd0                                            2:0    1    4K  0 disk
sda                                            8:0    0   60G  0 disk
├─sda1                                         8:1    0  256M  0 part /boot
└─sda2                                         8:2    0 59.8G  0 part
  ├─VolGroup00-rootLV                        253:0    0    8G  0 lvm  /
  ├─VolGroup00-swapLV                        253:1    0   16G  0 lvm  [SWAP]
  ├─VolGroup00-tmpLV                         253:2    0    1G  0 lvm  /tmp
  ├─VolGroup00-varLV-real                    253:3    0    4G  0 lvm
  │ ├─VolGroup00-varLV                       253:4    0    4G  0 lvm  /var
  │ └─VolGroup00-varLV_snap                  253:6    0    4G  0 lvm
  ├─VolGroup00-var_log_audit                 253:7    0  608M  0 lvm  /var/log/audit
  ├─VolGroup00-appsLV                        253:8    0    1G  0 lvm  /apps
  ├─VolGroup00-var_crash                     253:9    0    1G  0 lvm  /var/crash
  ├─VolGroup00-apps_nagios                   253:10   0  256M  0 lvm  /apps/nagios
  ├─VolGroup00-apps_ora                      253:11   0   20G  0 lvm  /apps/ora
  ├─VolGroup00-favapdbt01                    253:12   0  100M  0 lvm  /favapdbt01
  └─VolGroup00-favapdbt01--ora--fra          253:15   0   25G  0 lvm  /favapdbt01/ora/fra
sdb                                            8:16   0  140G  0 disk
├─VolGroup00-favapdbt01--ora                 253:13   0  160G  0 lvm  /favapdbt01/ora
├─VolGroup00-favapdbt01--ora--archivelog01   253:14   0   55G  0 lvm  /favapdbt01/ora/archivelog01
└─VolGroup00-favapdbt01--ora--fra            253:15   0   25G  0 lvm  /favapdbt01/ora/fra
sdc                                            8:32   0  100G  0 disk
└─sdc1                                         8:33   0  100G  0 part
  ├─VolGroup00-varLV_snap-cow                253:5    0  512M  0 lvm
  │ └─VolGroup00-varLV_snap                  253:6    0    4G  0 lvm
  ├─VolGroup00-favapdbt01--ora               253:13   0  160G  0 lvm  /favapdbt01/ora
  ├─VolGroup00-favapdbt01--ora--archivelog01 253:14   0   55G  0 lvm  /favapdbt01/ora/archivelog01
  ├─VolGroup00-favapdbt01--ora--fra          253:15   0   25G  0 lvm  /favapdbt01/ora/fra
  └─VolGroup00-OSbackup                      253:16   0  5.4G  0 lvm
sdd                                            8:48   0  100G  0 disk
sde                                            8:64   0  100G  0 disk
sr0              
```
