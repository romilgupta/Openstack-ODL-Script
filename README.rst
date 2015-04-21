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

References::
  https://wiki.opendaylight.org/view/OpenStack_and_OpenDaylight
  
