## Intro

Core has the ability to _impersonate_ other users, so that, once configured, when the impersonating user logins in, it will work, at any effect, as if the impersonated user was the one being log in. The system will remove this capability automatically on first sucessdful login, so on next login, it will be again it's own user

To impersonate another user, you must call API `impersonate` while you are logged in (or you provide your credentials to HTTP call) indicating the impersonated user. You must demostrate permission `impersonate` over the impersonated user, or any of his/her parents (office, organization, partner).





