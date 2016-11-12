ansible role: vm_build
========================

Ansible role to install base libvirt environment, which
allows to run qemu/kvm virtual machines.
This role relies on cloud init based images to be able 
inject meta-data via attached ISO for instance network
configuration.

It creates OpenStack config-drives data for nodes. 
It creates a basic configuration drive containing network 
configuration, a SSH key permitting the user to login to 
the host, and other files like `/etc/hosts` or 
`/etc/resolv.conf`. Also, it is able to include user_data 
file https://help.ubuntu.com/community/CloudInit



Configuration
-------------

Role parameters
```
---
- hosts: all
  become: true

  pre_tasks:
  roles:
    - role: .
      vm_build_name: "test"
      vm_build_cpu: 2
      vm_build_memory: 2048
      vm_build_disk: 20G
      vm_build_image: https://cloud-images.ubuntu.com/trusty/20161109/trusty-server-cloudimg-amd64-disk1.img
      vm_build_image_sum: md5:e7a5ba4699b1558a9021b04bd48ccbe0
      vm_build_virtualization: "qemu"
      vm_build_os_family: "Debian"
      vm_build_uuid: "ffab2cd3-b9a6-fafb-6887-d058688aa57f"
      vm_build_fqdn: "test.example.com"
      #vm_build_ssh_public_key_path: "/openstack/id_rsa.pub"
      vm_build_ssh_public_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDDvavjyPB+HLO7Ed5ZQT0ALx3aZU7BkII3a6BWeBP0Y40n8+P3kiaaOolPCjA4xPh0iqFyLGtTB9NFuj2NKaoEqE6be/8BHmPX00bYx+7iaJWVCPJuMH0lnBjRQYLCUC6MbrgEdNaI+KH7FDCEn9xDlJLHMrhriKVky8WOI7HVimlTuSKnwntvvAO9sd9V7sE0QGWA1sIJ1+w/sIK6YjRfE5KdyjYN0l8XaBSvhg0rn4TvxrwuM0iNB7xNw1yMKiHRcygMXHJFTPevO0ftSYHd/9biqzr4njWuFaGxsSXDO528lXzaXHJFzLrjqDlaRsmExK58j1EOFg/383bh0Rv root@qemu"
      vm_build_availability_zone: ""
      vm_build_network_info: True
      vm_build_config_dir: "/openstack/qemu/iso/config"
      vm_build_iso_dir: "/openstack/qemu/iso"
      vm_build_resolv:
        domain: "example.com"
        search: "demo.example.com"
        dns: ['8.8.8.8']
      vm_build_hosts:
        - ['127.0.1.1', 'host1.domain.com']
        - ['127.0.1.2', 'host2.domain.com']
      vm_build_network_device_list:
        - device: "eth0"
          host_net_type: "bridge" 
          host_net_dev: "virbr0"
          bootproto: "dhcp"
        - device: "eth1"
          host_net_type: "macvtap" 
          host_net_dev: "eth0"
          bootproto: "dhcp"
        - device: "eth2"
          host_net_type: "macvtap" 
          host_net_dev: "eth1"
          bootproto: "static"
          address: "10.1.1.10"
          netmask: "255.255.255.0"
          gateway: "10.1.1.1"
          nameservers: 
            - 8.8.8.8
            - 9.9.9.9
          domain: "domain.com"

#   - name: Display all variables/facts known for a host
#     debug: var=hostvars[inventory_hostname]
```
