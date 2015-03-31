# Tinc for virtual Proxmox VE cluster

This sets up a tinc vpn between several Proxmox VE servers. It adds the tun device named "vpn" to the Proxmox VE default bridge "vmbr0". In addition it will distribute the hosts file "roles/tinc/templates/hosts.j2" to /etc/hosts on each node. Currently the hosts within this file need to get configured manually. Eventually this should get build dynamically. No time yet to wrap my head around this one...

## Running

Create a hosts file. Add your hosts - one per line.

```bash
$ cat hosts
host1.my-domain:22
host2.my-domain:22
host3.my-domain:22
host4.my-domain:22
```

Next up, run ansible.

```bash
$ ansible-playbook site.yml
```

This will install tinc and delete any pre existing tinc configuration. In fact it will delete "/etc/tinc" before creating a new prestine configuration. This way each time the playbook runs new keys will get generated and distributed. By omitting old or obsolete nodes from the hosts file this effectively blocks them from reconnecting to the VPN. 

If that completes OK, you should be able to ping all Proxmox VE hosts with their internal IP which is set on vmbr0. For this to work all servers must reside within the same subnet.
