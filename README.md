<meta name="author" content="Marco Noce">
<meta name="description" content="Gathers facts about Oracle Exadata Machine">
<meta name="copyright" content="Marco Noce 2025">
<meta name="keywords" content="ansible, module, fact, oracle, exadata, imageinfo, dmidecode, exadata img hw, databasemachine.xml">

<div align="center">

![Ansible Custom Module][ansible-shield]
![Oracle Exadata][exadata-shield]
![python][python-shield]
![license][license-shield]

</div>


### exa_facts ansible custom module
#### Gathers facts Oracle Exadata Machine by imageinfo, exadata.img.hw, dmidecode and databasemachine.xml

#### Description :

<b>exa_facts</b> is an ansible custom module that creates and adds four dict to ansible_facts
* exa_img ( from <code>imageinfo</code> command )
  * Image image type
  * Kernel version
  * Image created
  * Image status
  * Uptrack kernel version
  * Node type
  * Image version
  * System partition on device
  * Image label
  * Image kernel version
  * Install type
  * Image activated
* exa_hw ( from <code>exadata.img.hw</code> command )
  * model
* system_info ( from <code>dmidecode</code> command )
  * SKU Number
  * UUID
  * Family
  * Serial Number
  * Version
  * Product Name
  * Wake-up Type
  * Manufacturer
* databasemachine ( from <code>/opt/oracle.SupportTools/onecommand/databasemachine.xml</code> file )
  * ORACLE_CLUSTER PAGES and RACK
    * MACHINETYPE
    * MACHINETYPES
    * RACKCOUNT
    * MACHINEUSIZE
    * ITEMS
      * RACK_SERIAL
      * ADMINNAME
      * ILOMIP
      * UHEIGHT
      * ILOMNAME
      * ULOCATION
      * TYPE
      * ADMINIP
      * CLIENTNAME
      * CLIENTIP

#### Repo files:

```
├── /library                
│   └── exa_facts.py   ##<-- python custom module
```

#### Requirements :

*  This module supports Oracle Exadata Machine
*  exa_img require [imageinfo] command
*  exa_hw require [exadata.img.hw] command
*  system_info require [dmidecode] command
*  databasemachine require [databasemachine.xml] file

#### Parameters :

*  no parameters are needed

#### Attributes :

|Attribute |Support|Description                                                                         |
|----------|-------|------------------------------------------------------------------------------------|
|check_mode|full   |Can run in check_mode and return changed status prediction without modifying target.|
|facts     |full   |Action returns an ansible_facts dictionary that will update existing host facts.    |

#### Examples dict in ansible_facts:
```json
    "exa_hw": {
      "model": "HVM domU"
    },
```
```json
    "exa_img": {
      "Image image type": "prod",
      "Kernel version": "4.14.35-2047.518.4.2.el7uek.x86_64 #2 SMP Thu Nov 3 14:28:31 PDT 2022 x86_64",
      "Image created": "2023-03-02 03:40:44 -0800",
      "Image status": "success",
      "Uptrack kernel version": "4.14.35-2047.522.3.el7uek.x86_64 #2 SMP Fri Jan 20 16:05:02 PST 2023 x86_64",
      "Node type": "GUEST",
      "Image version": "22.1.9.0.0.230302",
      "System partition on device": "/dev/mapper/VGExaDb-LVDbSys2",
      "Image label": "OSS_22.1.9.0.0_LINUX.X64_230302",
      "Image kernel version": "4.14.35-2047.518.4.2.el7uek",
      "Install type": "XEN Guest with InfiniBand",
      "Image activated": "2023-09-02 04:02:42 +0200"
    },
```
```json
    "system_info": {
      "SKU Number": "Not Specified",
      "UUID": "123456ba-b12f-1234-acce-be12a34fab56",
      "Family": "Not Specified",
      "Serial Number": "123456ba-b12f-1234-acce-be12a34fab5",
      "Version": "4.4.4OVM",
      "Product Name": "HVM domU",
      "Wake-up Type": "Power Switch",
      "Manufacturer": "Xen"
    },
```
#### this example has been simplified due to the amount of information in the databasemachine.xml file
```json
    "databasemachine": {
      "ORACLE_CLUSTER": {
        "PAGE0": {
          "RACKS": {
            "RACK": {
              "MACHINETYPE": "487",
              "MACHINEUSIZE": "42",
              "ITEMS": "13",
              "RACK_SERIAL": "00000000",
              "ITEM": [
                {
                  "ADMINNAME": "testceladm04",
                  "ILOMIP": "10.10.10.10",
                  "UHEIGHT": "2",
                  "ILOMNAME": "testceladm04-ilom",
                  "ULOCATION": "2",
                  "TYPE": "cellnode",
                  "ADMINIP": "11.11.11.11"
                },
                {
                  "ADMINNAME": "testdbvadm01",
                  "ILOMIP": "14.14.14.14",
                  "UHEIGHT": "1",
                  "ILOMNAME": "testdbvadm01-ilom",
                  "ULOCATION": "16",
                  "CLIENTIP": null,
                  "TYPE": "computenode",
                  "ADMINIP": "14.14.14.14",
                  "CLIENTNAME": null
                },
                {
                  "ADMINIP": "16.16.16.16",
                  "ULOCATION": "20",
                  "TYPE": "ib",
                  "ADMINNAME": "testsw-iba01",
                  "UHEIGHT": "1"
                },
                {
                  "ADMINIP": "18.18.18.18",
                  "ULOCATION": "21",
                  "TYPE": "cisco",
                  "ADMINNAME": "testsw-adm01",
                  "UHEIGHT": "1"
                },
                {
                  "ADMINIP": "13.13.13.13",
                  "ULOCATION": "0",
                  "TYPE": "pdu",
                  "ADMINNAME": "testsw-pdua01",
                  "UHEIGHT": "0"
                }
              ],
              "MACHINETYPES": "X5-2 Elastic Rack HC 4TB"
            }
          },
          "RACKCOUNT": "1"
        }
      }
    }
```
#### Tasks example
#### Gather info
```yaml
  - name: Gather exa info
    exa_facts:
```
#### Print exa hw model from exa_hw
```yaml
  - name: print exa hw model
    debug:
      msg: "Model: {{ ansible_facts.exa_hw.model }}"
```
#### Print exa Image version from exa_img
```yaml
  - name: print exa Image version
    debug:
      msg: "Image Version: {{ ansible_facts.exa_img['Image version'] }}"
```
#### Print exa serial number from system_info
```yaml
  - name: print exa serial number
    debug:
      msg: "Serial number: {{ ansible_facts.system_info['Serial Number'] }}"
```
#### Print cell list from databasemachine
```yaml
  - name: print cell list
    debug:
      msg: "{{ ansible_facts.databasemachine.ORACLE_CLUSTER.PAGE0.RACKS.RACK.ITEM | selectattr('TYPE', 'equalto', 'cellnode') | map(attribute='ADMINNAME') | list }}"
```
#### Print ib from databasemachine
```yaml
  - name: print ibswitch list 
    debug:
      msg: "{{ ansible_facts.databasemachine.ORACLE_CLUSTER.PAGE0.RACKS.RACK.ITEM | selectattr('TYPE', 'equalto', 'ib') | map(attribute='ADMINNAME') | list }}"
```
## SANITY TEST

* Ansible sanity test is available in [SANITY.md] file

## Integration

1. Assuming you are in the root folder of your ansible project.

Specify a module path in your ansible configuration file.

```shell
$ vim ansible.cfg
```
```ini
[defaults]
...
library = ./library
...
```

Create the directory and copy the python modules into that directory

```shell
$ mkdir library
$ cp path/to/module library
```

2. If you use Ansible AWX and have no way to edit the control node, you can add the /library directory to the same directory as the playbook .yml file

```
├── root repository
│   ├── playbooks
│   │    ├── /library                
│   │    │   └── exa_facts.py      ##<-- python custom module
│   │    └── your_playbook.yml     ##<-- you playbook
```   

[ansible-shield]: https://img.shields.io/badge/Ansible-custom%20module-blue?style=for-the-badge&logo=ansible&logoColor=lightgrey
[exadata-shield]: https://img.shields.io/badge/oracle-exadata-red?style=for-the-badge&logo=oracle&logoColor=red
[python-shield]: https://img.shields.io/badge/python-blue?style=for-the-badge&logo=python&logoColor=yellow
[license-shield]: https://img.shields.io/github/license/nomakcooper/svcs_facts?style=for-the-badge&label=LICENSE

[imageinfo]: https://docs.oracle.com/en/engineered-systems/exadata-database-machine/sagug/summary-software-and-firmware-components-oracle-exadata-storage-servers.html
[exadata.img.hw]: https://docs.oracle.com/en/engineered-systems/exadata-database-machine/dbmmn/determining-server-model.html
[dmidecode]: https://en.wikipedia.org/wiki/Dmidecode
[databasemachine.xml]: https://docs.oracle.com/cd/E24628_01/doc.121/e27442/ch3_discovery.htm
[SANITY.md]: SANITY.md
