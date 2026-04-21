# Ansible Collection - riotinto.servicenow

This Ansible collection provides custom modules to interact with Riotinto ServiceNow APIs via Azure API Gateway, enabling automation of CMDB queries and Change Management operations.

## Collection Structure

- **Modules**
  - `get_cmdb_records`: Queries CMDB tables in ServiceNow using REST API.
  - `get_azure_token`: Authenticates with Microsoft Azure AD to retrieve bearer tokens for use in subsequent API calls.
  - `standard_change`: Manages ServiceNow Standard Changes and Change Tasks (create, update, list, close).


## Requirements

- Python 3.6+
- `requests` Python library
- Network access to Azure AD and ServiceNow API Gateway endpoints
- An Azure AD App Registration with client ID and secret

## Installation

To install the collection:

```bash
ansible-galaxy collection install riotinto-servicenow-<version\>.tar.gz


## USAGE EXAMPLE ##
    - name: Get Azure token
      riotinto.servicenow.get_azure_token:
        tenant_id: "{{ tenant-id }}"
        client_id: "{{ client-id }}"
        client_secret: "{{ client-secret }}"
        scope: "https://apim-icc-syd-npe.riotinto.org/.default"
      register: token_result

    - name: Get CMDB Relationship Info
      riotinto.servicenow.get_cmdb_records:
        instance_url: "https://apim-icc-syd-npe.riotinto.org/test/servicenow/table/v1"
        auth_token: "{{ token_result.access_token }}"
        subscription_key: "{{ subscription_key }}"
        table: "cmdb_rel_ci"
        sysparm_query: "type.nameSTARTSWITHprovisioned^child.sys_class_name=cmdb_ci_os_template"
        sysparm_fields: "parent.name,child.name,type.name"
      register: cmdb_output

    - name: Display CMDB Results
      debug:
        var: cmdb_output.response
    
    - name: Create Standard Change
      riotinto.servicenow.standard_change:
        instance_url: "{{ baseUrl }}"
        auth_token: "{{ auth_token.access_token }}"
        subscription_key: "{{ apim_key }}"
        validate_certs: false
        action: create_change
        short_description: "Monthly patching - Linux servers"
        start_date: "{{ change_start }}"
        end_date: "{{ change_end }}"
      register: change

    - name: Create Implementation task
      riotinto.servicenow.standard_change:
        instance_url: "{{ baseUrl }}"
        auth_token: "{{ auth_token.access_token }}"
        subscription_key: "{{ apim_key }}"
        validate_certs: false
        action: create_task
        change_sys_id: "{{ change.result.sys_id }}"
        short_description: "Implement change"
        planned_start_date: "{{ change_start }}"
        planned_end_date: "{{ change_end }}"
        assigned_to: "test"

    - name: List change tasks
      riotinto.servicenow.standard_change:
        instance_url: "{{ baseUrl }}"
        auth_token: "{{ auth_token.access_token }}"
        subscription_key: "{{ apim_key }}"
        validate_certs: false
        action: list_tasks
        change_sys_id: "{{ change.result.sys_id }}"
      register: change_tasks

    - name: Move tasks to Work in Progress
      riotinto.servicenow.standard_change:
        instance_url: "{{ baseUrl }}"
        auth_token: "{{ auth_token.access_token }}"
        subscription_key: "{{ apim_key }}"
        validate_certs: false
        action: update_task
        change_sys_id: "{{ change.result.sys_id }}"
        task_sys_id: "{{ item.sys_id.value }}"
        state: 2
        work_notes: "Task moved to Work in Progress by Ansible"
      loop: "{{ change_tasks.result.result }}"
      when: item.state.value | int == 1
    
    - name: Close change tasks
      riotinto.servicenow.standard_change:
        instance_url: "{{ baseUrl }}"
        auth_token: "{{ auth_token.access_token }}"
        subscription_key: "{{ apim_key }}"
        validate_certs: false
        action: update_task
        change_sys_id: "{{ change.result.sys_id }}"
        task_sys_id: "{{ item.sys_id.value }}"
        state: 3
        close_code: "Successful"
        work_notes: "Task closed automatically by Ansible"
      loop: "{{ change_tasks.result.result }}"
      when: item.state.value | int == 2

    - name: Move change to closed state
      riotinto.servicenow.standard_change:
        instance_url: "{{ baseUrl }}"
        auth_token: "{{ auth_token.access_token }}"
        subscription_key: "{{ apim_key }}"
        validate_certs: false
        close_code: successful  # Mandory Filed if "successful"
        action: update_change
        sys_id: "{{ change_sys_id }}"
        state: "{{ state.split(' ')[0] | int }}"
        work_notes: "Change moved to closed successful by Ansible User"

#####################################################################################################

## Module Details

### `get_azure_token`

Generates a bearer token from Azure AD.

**Parameters:**

- `tenant_id`: *(required)* Azure AD tenant ID  
- `client_id`: *(required)* Application (client) ID  
- `client_secret`: *(required)* Client secret  
- `scope`: *(required)* Scope of the token (e.g., API Gateway endpoint with `.default`)  

---

### `get_cmdb_records`

Fetches records from the ServiceNow CMDB API using the provided token and optional filters.

**Parameters:**

- `instance_url`: *(required)* Base URL of ServiceNow API  
- `auth_token`: *(required)* Bearer token for authorization  
- `subscription_key`: *(optional)* API management key for Azure Gateway  
- `table`: *(required)* CMDB table name (e.g., `cmdb_ci_server`)  
- `sysparm_query`: *(optional)* ServiceNow-style query string  
- `sysparm_fields`: *(optional)* Comma-separated fields to return  

---

### `standard_change`

Create a Standard Change using Template id. This module includes create/update tasks & Change. it also allows you to add work-notes

**Supported Actions:**
- `create_change`
- `get_change`
- `update_change`
- `create_task`
- `get_task`
- `list_tasks`
- `update_task`
- `get_template_id`

**Parameters:**

- `instance_url`: *(required)* APIM URL  
- `auth_token`: *(required)* Bearer token for authorization   
- `subscription_key`: *(required)* API management key for Azure Gateway 
- `action`: *(required)* create_change, get_change, update_change, list_tasks, get_task, create_task, update_task 
- `standard_change_template_id`: *(required)* change template id or sys id  
- `short_description`: *(optional)* change title  
- `cmdb_ci`: *(required)* CI of change
- `assigned_to`: *(required)* change assigned to 
- `justification`: *(optional)* justification of change 
- `risk_impact_analysis`: *(optional)* risk and impact
- `change_plan`: *(optional)* change plan  
- `start_date`: *(required)* change start date
- `end_date`: *(required)* change end date
- `validate_certs`: *(required)* false always

---

## License

**Proprietary** – Rio Tinto Internal Use Only

---

## Author Information

**Developed by:**  
R.GajananKahu@riotinto.com

**Contact:**  
RTISTAutomation@riotinto.com
