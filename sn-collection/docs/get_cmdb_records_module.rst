.. _riotinto.servicenow.get_cmdb_records:

**************************
riotinto.servicenow.get_cmdb_records
**************************

**Query CMDB tables in ServiceNow using REST API.**

Version added: 1.1.0

.. contents::
   :local:
   :depth: 1

Synopsis
--------
- Queries ServiceNow CMDB tables via Azure API Gateway.
- Supports filtering and pagination.

Parameters
----------

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr>
            <th>Parameter</th><th>Choices/<font color="blue">Defaults</font></th><th>Description</th>
        </tr>
        <tr>
            <td><b>instance_url</b><div style="font-size: small"><span style="color: purple">string</span> / <span style="color: red">required</span></div></td>
            <td></td>
            <td>Base URL of the ServiceNow instance (via API Gateway).</td>
        </tr>
        <tr>
            <td><b>token</b><div style="font-size: small"><span style="color: purple">string</span> / <span style="color: red">required</span></div></td>
            <td></td>
            <td>Bearer token (from get_azure_token).</td>
        </tr>
        <tr>
            <td><b>table</b><div style="font-size: small"><span style="color: purple">string</span> / <span style="color: red">required</span></div></td>
            <td></td>
            <td>Name of the CMDB table (e.g., cmdb_ci_server).</td>
        </tr>
        <tr>
            <td><b>query</b><div style="font-size: small"><span style="color: purple">string</span></div></td>
            <td></td>
            <td>Encoded query string for filtering records.</td>
        </tr>
        <tr>
            <td><b>limit</b><div style="font-size: small"><span style="color: purple">integer</span></div></td>
            <td>100</td>
            <td>Maximum number of records to fetch.</td>
        </tr>
    </table>

Examples
--------

.. code-block:: yaml

    - name: Get all server records from CMDB
      riotinto.servicenow.get_cmdb_records:
        instance_url: "https://apigateway.company.com/servicenow"
        token: "{{ auth_token.access_token }}"
        table: "cmdb_ci_server"
        limit: 50
      register: cmdb_records

Return Values
-------------

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr><th>Key</th><th>Returned</th><th>Description</th></tr>
        <tr>
            <td><b>records</b><div style="font-size: small"><span style="color: purple">list</span></div></td>
            <td>success</td>
            <td>List of CMDB records retrieved from ServiceNow.</td>
        </tr>
        <tr>
            <td><b>count</b><div style="font-size: small"><span style="color: purple">integer</span></div></td>
            <td>success</td>
            <td>Number of records returned.</td>
        </tr>
    </table>
