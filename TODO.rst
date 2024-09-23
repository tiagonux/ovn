..
      Licensed under the Apache License, Version 2.0 (the "License"); you may
      not use this file except in compliance with the License. You may obtain
      a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

      Unless required by applicable law or agreed to in writing, software
      distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
      WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
      License for the specific language governing permissions and limitations
      under the License.

      Convention for heading levels in OVN documentation:

      =======  Heading 0 (reserved for the title in a document)
      -------  Heading 1
      ~~~~~~~  Heading 2
      +++++++  Heading 3
      '''''''  Heading 4

      Avoid deeper levels because they do not render well.

==============
OVN To-do List
==============

* Refactor ovn-northd code to have separate functions to add logical flows
  for gateway logical routers and logical routers with distributed gateway
  port.

* VLAN trunk ports.

  Russell Bryant: "Today that would require creating 4096 ports for the VM and
  attach to 4096 OVN networks, so doable, but not quite ideal."

* Service function chaining.

* Hitless upgrade, especially for data plane.

* Dynamic IP to MAC binding enhancements.

  OVN has basic support for establishing IP to MAC bindings dynamically, using
  ARP.

  * Table size limiting.

    The table of MAC bindings must not be allowed to grow unreasonably large.

* MTU handling (fragmentation on output)

* Support multiple tunnel encapsulations in Chassis.

  So far, both ovn-controller and ovn-controller-vtep only allow chassis to
  have one tunnel encapsulation entry.  We should extend the implementation
  to support multiple tunnel encapsulations.

* Update learned MAC addresses from VTEP to OVN

  The VTEP gateway stores all MAC addresses learned from its physical
  interfaces in the 'Ucast_Macs_Local' and the 'Mcast_Macs_Local' tables.
  ovn-controller-vtep should be able to update that information back to
  ovn-sb database, so that other chassis know where to send packets destined
  to the extended external network instead of broadcasting.

* Translate ovn-sb Multicast_Group table into VTEP config

  The ovn-controller-vtep daemon should be able to translate the
  Multicast_Group table entry in ovn-sb database into Mcast_Macs_Remote table
  configuration in VTEP database.

* OVN OCF pacemaker script to support Active / Passive HA for OVN dbs provides
  the option to configure the inactivity_probe value. The default 5 seconds
  inactivity_probe value is not sufficient and ovsdb-server drops the client
  IDL connections for openstack deployments when the neutron server is heavily
  loaded.

  We need to find a proper solution to solve this issue instead of increasing
  the inactivity_probe value.

* ACL

  * Support FTP ALGs.

* OVN Interconnection

  * Packaging for Debian.

* IP Multicast Relay

  * When connecting bridged logical switches (localnet) to logical routers
    with IP Multicast Relay enabled packets might get duplicated. We need
    to find a way of determining if routing has already been executed (on a
    different hypervisor) for the IP multicast packet being processed locally
    in the router pipeline.

* ovn-controller Incremental processing

  * Implement I-P for datapath groups.

* ovn-northd parallel logical flow processing

  * Multi-threaded logical flow computation was optimized for the case
    when datapath groups are disabled.  Datpath groups are always enabled
    now so northd parallel processing should be revisited.

* ovn-controller daemon module

  * Dumitru Ceara: Add a new module e.g. ovn/lib/daemon-ovn.c that wraps
    OVS' daemonize_start() call and initializes the additional things, like
    the unixctl commands. Or, we should move the APIs such as
    daemon_started_recently() to OVS's lib/daemon.

* Chassis_Template_Var

  * Support template variables when tracing packets with ovn-trace.

* Load Balancer templates

  * Support combining the VIP IP and port into a single template variable.

* ovn-controller conditional monitoring

  * Improve sub-ports (with parent_port set) conditional monitoring; these
    are currently unconditionally monitored, even if ovn-monitor-all is
    set to false.

* ovn-northd parallel build

  * Move the lflow build parallel processing from northd.c to lflow-mgr.c
    This would also ensure that the variables declared in northd.c
    (eg. thread_lflow_counter) are not externed in lflow-mgr.c.

* Remove flows with `check_pkt_larger` when userspace datapath can handle
  PMTUD. (https://issues.redhat.com/browse/FDP-256)
