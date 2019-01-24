Ansible Role: oracle-gatherinfo-databases
=========================================

This role automatically discovers Oracle Homes and gathers data about registered Oracle databases.

Collected information is stored in role's `oracle_gatherinfo_databases_oracle_databases` variable and global `oracle_databases` variable.

Sample output:

    "oracle_databases": [
        {
            "cluster_database": "false",
            "database_role": "PRIMARY",
            "database_type": "SINGLE",
            "db_name": "ORCL",
            "db_unique_name": "ORCL",
            "edition": "Enterprise",
            "instance_name": "ORCL",
            "instances": "ORCL",
            "is_registered_in_gi": "true",
            "oracle_home": "/u01/app/oracle/product/11.2.0.4/dbhome1",
            "oracle_home_patches": [
                {
                    "patch_description": "OJVM PATCH SET UPDATE 11.2.0.4.181016",
                    "patch_number": "28440700"
                },
                {
                    "patch_description": "Database Patch Set Update : 11.2.0.4.181016 (28204707)",
                    "patch_number": "28204707"
                }
            ],
            "software_version": "11.2.0.4.0"
        }
    ]


Supported OS:
-------------
* RedHat
* CentOS
* OracleLinux

Requirements
------------

This role uses `oracle` and `oracle-gatherinfo-gi` roles.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):


    # source of database facts - Oracle Grid Infrastructure registry (whether to collect info from GI or not)
    oracle_gatherinfo_databases_from_gi_registry: true

    # List describing registered Oracle databases (dynamically populated)
    oracle_gatherinfo_databases_oracle_databases: []

    # e.g.:
    #oracle_gatherinfo_databases_oracle_databases:
    #  - cluster_database: "false"
    #    database_role: "PRIMARY"
    #    database_type: "SINGLE"
    #    db_name: "ORCL"
    #    db_unique_name: "ORCL"
    #    edition: "Enterprise"
    #    instance_name: "ORCL"
    #    instances: "ORCL"
    #    is_registered_in_gi: "true"
    #    oracle_home: "/u01/app/oracle/product/11.2.0.4/dbhome1"
    #    software_version: "11.2.0.4.0"

    # print collected information about databases (display oracle_gatherinfo_databases_oracle_databases variable)
    oracle_gatherinfo_databases_print_collected_data: true

    # get ORACLE_HOME patches
    oracle_gatherinfo_databases_get_oracle_home_patches: false

    # get extra facts (for example for capacity report)
    oracle_gatherinfo_databases_get_extra_facts: false

    # facts directory
    oracle_gatherinfo_databases_facts_dir: "{{ oracle_facts_dir | default('~/scripts/facts.d', true) }}"

    # a list of fact scripts templates used by Ansible 'setup' module
    oracle_gatherinfo_databases_fact_script_templates:
      - oracle_databases.fact


Dependencies
------------

This role uses `oracle` role.

Example Playbook
----------------

    - name: Gather information about installed Oracle components
      hosts: ora-servers
      gather_facts: true
      become: true
      become_user: '{{ oracle_user }}'

      tasks:

      - import_role:
          name: oracle-gatherinfo-databases
        tags:
          - oracle-gatherinfo-databases
          - oracle-gatherinfo-allcomponents

		  
Inside `vars/main.yml` or `group_vars/..` or `host_vars/..`:

    
    #-------------------------------------------------------
    # overrides role 'oracle-gatherinfo-databases' variables
    #-------------------------------------------------------

    # get ORACLE_HOME patches
    oracle_gatherinfo_databases_get_oracle_home_patches: true

    # get extra facts (for example for capacity report)
    oracle_gatherinfo_databases_get_extra_facts: true
		
    # ... etc ...


License
-------

GPLv3 - GNU General Public License v3.0

Author Information
------------------

This role was created in 2018 by [Krzysztof Lewandowski](mailto:Krzysztof.Lewandowski@fastmail.fm).


