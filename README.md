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
└── play_test.yml      ##<-- ansible playbook example
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

#### Examples :

#### Tasks example
#### Gather info
```yaml
  - name: Gather exa info
    exa_facts:
```
#### svcs_list facts:
```json
"ansible_facts": {
    "svcs_list": [
      {
        "STATE": "disabled",
        "NSTATE": "-",
        "STIME": "21:21:57",
        "CTID": "67",
        "FMRI": "svc:/platform/i86pc/acpihpd:default"
      }
  ]
},
```
#### debug output from example :
```
TASK [print STATE of sendmail service] *****************************************
ok: [sol11host] => {
    "msg": "sendmail service is in online state from 21:22:06"
}
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
│   │    │   └── svcs_facts.py      ##<-- python custom module
│   │    └── your_playbook.yml      ##<-- you playbook
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
