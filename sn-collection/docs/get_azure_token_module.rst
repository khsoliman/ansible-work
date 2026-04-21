.. _riotinto.servicenow.get_azure_token:

**********************
riotinto.servicenow.get_azure_token
**********************

**Authenticate with Microsoft Azure AD to retrieve bearer tokens for API calls.**

Version added: 1.1.0

.. contents::
   :local:
   :depth: 1

Synopsis
--------
- Authenticates with Microsoft Azure AD.
- Returns a bearer token for use in ServiceNow API calls via Azure API Gateway.

Parameters
----------

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr>
            <th>Parameter</th><th>Choices/<font color="blue">Defaults</font></th><th>Description</th>
        </tr>
        <tr>
            <td><b>tenant_id</b><div style="font-size: small"><span style="color: purple">string</span> / <span style="color: red">required</span></div></td>
            <td></td>
            <td>Azure AD tenant ID.</td>
        </tr>
        <tr>
            <td><b>client_id</b><div style="font-size: small"><span style="color: purple">string</span> / <span style="color: red">required</span></div></td>
            <td></td>
            <td>Azure AD application client ID.</td>
        </tr>
        <tr>
            <td><b>client_secret</b><div style="font-size: small"><span style="color: purple">string</span> / <span style="color: red">required</span></div></td>
            <td></td>
            <td>Azure AD application client secret.</td>
        </tr>
        <tr>
            <td><b>resource</b><div style="font-size: small"><span style="color: purple">string</span></div></td>
            <td><div style="color: blue">https://management.azure.com</div></td>
            <td>Resource for which token is requested.</td>
        </tr>
    </table>

Examples
--------

.. code-block:: yaml

    - name: Get Azure AD token
      riotinto.servicenow.get_azure_token:
        tenant_id: "xxxxx"
        client_id: "xxxxx"
        client_secret: "xxxxx"
        resource: "https://management.azure.com"
      register: auth_token

Return Values
-------------

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr><th>Key</th><th>Returned</th><th>Description</th></tr>
        <tr>
            <td><b>access_token</b><div style="font-size: small"><span style="color: purple">string</span></div></td>
            <td>success</td>
            <td>Bearer token for ServiceNow API authentication.</td>
        </tr>
        <tr>
            <td><b>expires_on</b><div style="font-size: small"><span style="color: purple">string</span></div></td>
            <td>success</td>
            <td>Expiration timestamp of token.</td>
        </tr>
    </table>
