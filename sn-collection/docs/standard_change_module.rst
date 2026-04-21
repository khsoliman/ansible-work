.. _riotinto.servicenow.standard_change:

******************************
riotinto.servicenow.standard_change
******************************

**Manage ServiceNow Standard Changes and Change Tasks via Azure API Gateway.**

Version added: 1.1.0

.. contents::
   :local:
   :depth: 1

Synopsis
--------
- Create, retrieve, and update ServiceNow Standard Changes.
- Create, retrieve, update, and list Change Tasks.
- Fetch Standard Change Template IDs.
- Add work notes during change or task updates.
- Uses Azure AD authentication and Azure API Management endpoints.

Parameters
----------

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr>
            <th>Parameter</th><th>Choices/<font color="blue">Defaults</font></th><th>Description</th>
        </tr>

        <tr>
            <td><b>instance_url</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span> /
                    <span style="color: red">required</span>
                </div>
            </td>
            <td></td>
            <td>Azure API Management endpoint for ServiceNow.</td>
        </tr>

        <tr>
            <td><b>auth_token</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span> /
                    <span style="color: red">required</span>
                </div>
            </td>
            <td></td>
            <td>Bearer token used for API authentication.</td>
        </tr>

        <tr>
            <td><b>subscription_key</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span> /
                    <span style="color: red">required</span>
                </div>
            </td>
            <td></td>
            <td>Azure API Management subscription key.</td>
        </tr>

        <tr>
            <td><b>action</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span> /
                    <span style="color: red">required</span>
                </div>
            </td>
            <td>
                create_change<br>
                get_change<br>
                update_change<br>
                create_task<br>
                get_task<br>
                list_tasks<br>
                update_task<br>
                get_template_id
            </td>
            <td>Action to perform on ServiceNow Standard Change or Change Task.</td>
        </tr>

        <tr>
            <td><b>standard_change_template_id</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Standard Change Template sys_id. Required for <b>create_change</b>.</td>
        </tr>

        <tr>
            <td><b>short_description</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Short description or title of the change.</td>
        </tr>

        <tr>
            <td><b>cmdb_ci</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Configuration Item (CI) associated with the change.</td>
        </tr>

        <tr>
            <td><b>assigned_to</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>User assigned to the change or task.</td>
        </tr>

        <tr>
            <td><b>justification</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Business justification for the change.</td>
        </tr>

        <tr>
            <td><b>risk_impact_analysis</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Risk and impact analysis details.</td>
        </tr>

        <tr>
            <td><b>change_plan</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Implementation plan for the change.</td>
        </tr>

        <tr>
            <td><b>start_date</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Change start date and time.</td>
        </tr>

        <tr>
            <td><b>end_date</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td></td>
            <td>Change end date and time.</td>
        </tr>

        <tr>
            <td><b>validate_certs</b>
                <div style="font-size: small">
                    <span style="color: purple">bool</span>
                </div>
            </td>
            <td><div style="color: blue">false</div></td>
            <td>SSL certificate validation. Must be set to false.</td>
        </tr>
    </table>

Examples
--------

Create a Standard Change
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: yaml

    - name: Create Standard Change
      riotinto.servicenow.standard_change:
        instance_url: "https://apim.example.org/servicenow/change"
        auth_token: "{{ token.access_token }}"
        subscription_key: "{{ subscription_key }}"
        action: create_change
        standard_change_template_id: "abcd1234efgh5678"
        short_description: "OS Patching"
        cmdb_ci: "server01"
        assigned_to: "john.doe"
        start_date: "2026-02-10 10:00:00"
        end_date: "2026-02-10 12:00:00"
        validate_certs: false

Get Change Tasks
~~~~~~~~~~~~~~~~

.. code-block:: yaml

    - name: List Change Tasks
      riotinto.servicenow.standard_change:
        instance_url: "https://apim.example.org/servicenow/change"
        auth_token: "{{ token.access_token }}"
        subscription_key: "{{ subscription_key }}"
        action: list_tasks
        validate_certs: false

Return Values
-------------

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr>
            <th>Key</th><th>Returned</th><th>Description</th>
        </tr>
        <tr>
            <td><b>response</b>
                <div style="font-size: small">
                    <span style="color: purple">dict</span>
                </div>
            </td>
            <td>success</td>
            <td>ServiceNow API response containing change or task details.</td>
        </tr>
        <tr>
            <td><b>status</b>
                <div style="font-size: small">
                    <span style="color: purple">string</span>
                </div>
            </td>
            <td>success</td>
            <td>Status of the requested operation.</td>
        </tr>
    </table>
