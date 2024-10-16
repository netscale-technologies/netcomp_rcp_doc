## User Impersonation Feature in Core System

The Core system provides a feature that allows one user to _impersonate another_. Once configured, when the impersonating user logs in, the system will treat the session as though the impersonated user is the one logging in, with full access and functionality equivalent to the impersonated user’s profile. This impersonation capability is automatically revoked upon the first successful login, ensuring that subsequent logins will revert to the original user’s credentials.

## How to Impersonate Another User:

To initiate impersonation, the currently logged-in user must invoke the impersonate API. This API call can be made during an active session or by providing valid credentials within the HTTP request. The request must specify the user to be impersonated. Additionally, the invoking user must have the appropriate impersonate permission for the targeted user or any of the user’s hierarchical entities (e.g., office, organization, partner).

### impersonate

|Field|Type|Mandatory|Description
|---|---|---|---
|user_uid|string|N|Impersonating user. It will use current logged-in user by default
|target_uid|string|N|Impersonated user. If not provided it will remove the impersonating sessions

If successful, user should log out, and on next login if will be impersonating. 
On login request, new field `impersonating_uid` will be returned, so UI can check if this is indeed an impersonating session
