# Collections Plugins Directory

This directory can be used to ship various plugins inside an Ansible collection. Each plugin is placed in a folder that
is named after the type of plugin it is in. It can also include the `module_utils` and `modules` directory that would contain module utils and modules respectively.

Here is an example directory of the majority of plugins currently supported by Ansible:

```
└── plugins
    ├── modules
```

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


A full list of plugin types can be found at [Working With Plugins](https://docs.ansible.com/ansible-core/2.16/plugins/plugins.html).
