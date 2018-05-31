CentOS Tuning 
=========

Ansible role to properly tune CentOS server(s) after minimal installation.
- Configure hostname(s) 
- Configure yum repos (optional)
- Updates all packages to latest 
- Install some tools (TODO)
- Tuning system (WIP)
- Configure SSH & NTP (TODO)

Requirements
------------

TODO

Role Variables
--------------

```yaml
# This is default variables
host_role_name: host_prepare_tuning

host_need_reboot: false
host_reboot_timeout: 600

host_is_tuning: false
host_sysctl_vars:
  - { regexp: '^fs\.file-max \= ', line: 'fs.file-max = 1000000' } # max number of open files (sockets are files they provide bytes via read/write interface)
  - { regexp: '^net\.ipv4\.tcp_tw_reuse \= ', line: 'net.ipv4.tcp_tw_reuse = 1' } #  reuse and recycle TIME_WAIT sockets
  - { regexp: '^net\.ipv4\.tcp_tw_recycle \= ', line: 'net.ipv4.tcp_tw_recycle = 1' } # NOTE: load balancing servers can have issues with this option enabled
  - { regexp: '^net\.ipv4\.tcp_max_syn_backlog \= ', line: 'net.ipv4.tcp_max_syn_backlog = 65535' } # max number of queued conn req (outstanding syn reqs)
  - { regexp: '^net\.ipv4\.ip_local_port_range \= ', line: 'net.ipv4.ip_local_port_range = 1024 65000' } # min / max port range
  - { regexp: '^net\.ipv4\.tcp_max_tw_buckets \= ', line: 'net.ipv4.tcp_max_tw_buckets = 400000' } # max number of TIME_WAIT sockets
  - { regexp: '^net\.ipv4\.tcp_no_metrics_save \= ', line: 'net.ipv4.tcp_no_metrics_save = 1' } # 
  - { regexp: '^net\.ipv4\.tcp_syn_retries \= ', line: 'net.ipv4.tcp_syn_retries = 2' } # num of retries to send SYN packet
  - { regexp: '^net\.ipv4\.tcp_synack_retries \= ', line: 'net.ipv4.tcp_synack_retries = 2' } # num of retries to send SYN,ACK reply
  - { regexp: '^net\.ipv4\.tcp_rmem \= ', line: 'net.ipv4.tcp_rmem = 4096 87380 16777216' } # min / default / max receieve buffer sizes
  - { regexp: '^net\.ipv4\.tcp_wmem \= ', line: 'net.ipv4.tcp_wmem = 4096 65536 16777216' } # min / default / max send buffer sizes
  - { regexp: '^net\.core\.somaxconn \= ', line: 'net.core.somaxconn = 65535' } # max number of requests queued for any given listening socket
  - { regexp: '^net\.core\.netdev_max_backlog \= ', line: 'net.core.netdev_max_backlog = 4096' } # max number of packets queued on interface end (INPUT side)
  - { regexp: '^net\.core\.rmem_max \= ', line: 'net.core.rmem_max = 16777216' } # max receieve socket buffer
  - { regexp: '^net\.core\.wmem_max \= ', line: 'net.core.wmem_max = 16777216' } # max send socket buffer
  - { regexp: '^net\.nf_conntrack_max \= ', line: 'net.nf_conntrack_max = 1048576' } # size of connection tracking table
  - { regexp: '^vm\.dirty_background_ratio \= ', line: 'vm.dirty_background_ratio = 10' }  #  memory/swap settings
  - { regexp: '^vm\.dirty_ratio \= ', line: 'vm.dirty_ratio = 40' } #  memory/swap settings
  - { regexp: '^vm\.swappiness \= ', line: 'vm.swappiness = 20' } #  memory/swap settings
host_limits_vars:
  - { regexp: '^\* soft nofile ', line: '* soft nofile 1000000' } 
  - { regexp: '^\* hard nofile ', line: '* hard nofile 1000000' }
  - { regexp: '^\* soft nproc ', line: '* soft nproc 393216' }
  - { regexp: '^\* hard nproc ', line: '* hard nproc 393216' }

Dependencies
------------

None
