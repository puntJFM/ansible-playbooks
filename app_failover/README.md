# Planned Switchover AAP Playbook

## Purpose

This playbook orchestrates a planned switchover flow using Ansible Automation Platform.

At a high level it:

1. validates incoming extra vars
2. records precheck / change activity
3. retrieves privileged credentials from CyberArk CCP
4. resolves environment-specific AutoSys settings
5. pauses scheduler activity
6. promotes the requested instance to primary
7. updates the internal system of record
8. resumes scheduler activity
9. records completion or failure

## Expected extra vars

Example:

```json
{
  "operation": "switchover",
  "target_environment": "PRD",
  "skylight_testing": false,
  "controller_username": "{{ lookup('env', 'controller_username') }}",
  "controller_password": "{{ lookup('env', 'controller_password') }}",
  "controller_host": "{{ lookup('env', 'controller_url') }}",
  "ccp_password": "{{ lookup('env', 'ccp_password') }}",
  "ccp_app_id": "{{ lookup('env', 'ccp_app_id') }}",
  "ccp_username": "INTRANET\\{{ lookup('env', 'ccp_username') }}",
  "ccp_url": "{{ lookup('env', 'ccp_url') }}",
  "ca_safe": "DB-Oracle-AIM",
  "dbi_api_url": "{{ lookup('env', 'dbi_api_url') }}",
  "dbi_api_authkey": "{{ lookup('env', 'dbi_api_auth_token') }}",
  "autosys_config": {
    "UAT": {
      "SERVER1": "autosys-devuat-cli.myserverint.com:8080",
      "SERVER2": "autosys-devuat-cli.myserverint.com:8080",
      "AUTOSERV": "U12"
    },
    "PRD": {
      "SERVER1": "autosys-prdldn-cli.myserverint.com:8080",
      "SERVER2": "autosys-prdldn-cli.myserverint.com:8080",
      "AUTOSERV": "P21"
    }
  },
  "autosys_cli_path": "/apps/MyDomainAutoSysCLI/bin",
  "autosys_cli_host": "gbrdsr00052n01.intranet.myserverint.com",
  "sf_ticket": "CHG123456",
  "parameter_check": "No",
  "aap_var_row_id": 0,
  "new_primary_instance_list": "CTICPZ10P"
}
