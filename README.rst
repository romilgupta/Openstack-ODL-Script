====================================================================
Openstack with Opendaylight Installation script for Ubuntu 14.04 LTS
====================================================================

Single Node Openstack Juno :
--------------------------

  Operating System : Ubuntu14.04 LTS

  NIC's::

    Eth0: Public Network/Management Network
    Eth1: Data Network

Download the Openstack-ODL-Script::
  
  git clone https://github.com/romilgupta/Openstack-ODL-Script.git
  cd Openstack-ODL-Script
  
Run ``python install_openstack.py``

Script will prompt you to enter following inputs::

  raw_input("Management Interface IP: ")
  raw_input("Data Interface IP: ")
  raw_input("OpenDaylight Controller IP: ")
  raw_input("Offline Mode True|False: ") # Provide False when you are runnning it first time.

The script will install following components of openstack and configure them::

  Keystone
  Glance
  Neutron(neutron-server with Opendaylight, dhcp-agent, l3-agent)
  Openvswitch
  Nova(nova-api nova-cert nova-scheduler nova-conductor novnc nova-consoleauth nova-novncproxy, nova-compute)
  Dashboard


==============================================
Opendaylight Installation for Ubuntu 14.04 LTS
==============================================

Follow the given steps::

  sudo apt-get install maven openjdk-7-jre openjdk-7-jdk

  vi ~/.bashrc
  export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-i386

  # Helium Stable Distribution Artifacts
  wget https://nexus.opendaylight.org/content/groups/public/org/opendaylight/integration/distribution-karaf/0.2.3-Helium-SR3/distribution-karaf-0.2.3-Helium-SR3.zip
  unzip distribution-karaf-0.2.3-Helium-SR3.zip
  cd distribution-karaf-0.2.3-Helium-SR3
  bin/karaf 
  
  feature:install odl-ovsdb-openstack odl-dlux-core
  feature:install odl-dlux-all odl-restconf odl-l2switch-switch

Troubleshooting:

sample output's::
  
  sudo ovs-vsctl show
  
  ad36014f-3918-402f-9b5f-d89c7b5096c4
    Manager "tcp:192.168.1.63:6640"
        is_connected: true
    Bridge br-int
        Controller "tcp:192.168.1.63:6633"
            is_connected: true
        Port "tap5e06e4bf-10"
            Interface "tap5e06e4bf-10"
        Port br-int
            Interface br-int
                type: internal
        Port "tap0700642e-c4"
            Interface "tap0700642e-c4"
                type: internal
    ovs_version: "2.0.2"

  sudo ovs-ofctl dump-flows br-int --protocols=OpenFlow13
  
  OFPST_FLOW reply (OF1.3) (xid=0x2):
  cookie=0x0, duration=316.283s, table=0, n_packets=0, n_bytes=0, priority=8192,in_port=1 actions=drop
  cookie=0x0, duration=182.731s, table=0, n_packets=0, n_bytes=0, priority=8192,in_port=2 actions=drop
  cookie=0x0, duration=1215.55s, table=0, n_packets=0, n_bytes=0, dl_type=0x88cc actions=CONTROLLER:65535
  cookie=0x2b00000000000003, duration=1215.555s, table=0, n_packets=0, n_bytes=0, priority=100,dl_type=0x88cc actions=CONTROLLER:65535
  cookie=0x0, duration=1214.591s, table=0, n_packets=8, n_bytes=636, priority=0 actions=goto_table:20
  cookie=0x0, duration=316.755s, table=0, n_packets=18, n_bytes=2042, in_port=1,dl_src=fa:16:3e:5e:a8:28 actions=set_field:0x1->tun_id,load:0x1->NXM_NX_REG0[],goto_table:20
  cookie=0x0, duration=183.242s, table=0, n_packets=125, n_bytes=6318, in_port=2,dl_src=fa:16:3e:d6:b6:20 actions=set_field:0x1->tun_id,load:0x1->NXM_NX_REG0[],goto_table:20
  cookie=0x0, duration=1214.089s, table=20, n_packets=149, n_bytes=8828, priority=0 actions=goto_table:30
  cookie=0x0, duration=1213.586s, table=30, n_packets=147, n_bytes=8668, priority=0 actions=goto_table:40
  cookie=0x0, duration=1213.083s, table=40, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:50
  cookie=0x0, duration=1212.574s, table=50, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:60
  cookie=0x0, duration=1212.072s, table=60, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:70
  cookie=0x0, duration=1211.569s, table=70, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:80
  cookie=0x0, duration=1211.065s, table=80, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:90
  cookie=0x0, duration=1210.562s, table=90, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:100
  cookie=0x0, duration=1210.059s, table=100, n_packets=146, n_bytes=8578, priority=0 actions=goto_table:110
  cookie=0x0, duration=313.621s, table=110, n_packets=0, n_bytes=0, priority=8192,tun_id=0x1 actions=drop
  cookie=0x0, duration=314.639s, table=110, n_packets=124, n_bytes=6176, priority=16384,reg0=0x1,tun_id=0x1,dl_dst=01:00:00:00:00:00/01:00:00:00:00:00 actions=output:1,output:2
  cookie=0x0, duration=315.126s, table=110, n_packets=0, n_bytes=0, priority=16384,reg0=0x2,tun_id=0x1,dl_dst=01:00:00:00:00:00/01:00:00:00:00:00 actions=output:1,output:2
  cookie=0x0, duration=315.778s, table=110, n_packets=6, n_bytes=532, tun_id=0x1,dl_dst=fa:16:3e:5e:a8:28 actions=output:1
  cookie=0x0, duration=182.231s, table=110, n_packets=9, n_bytes=1304, tun_id=0x1,dl_dst=fa:16:3e:d6:b6:20 actions=output:2
  cookie=0x0, duration=1209.555s, table=110, n_packets=6, n_bytes=496, priority=0 actions=drop

References:

  https://wiki.opendaylight.org/view/OpenStack_and_OpenDaylight
  
