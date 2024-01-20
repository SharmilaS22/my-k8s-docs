
Authentication
Authorization
Network policies


Authentication:
- Admins
- Developers
- End Users - app security
- Bots


User -> admin, developer
Service Account -> bots

Auth mechanisms:
1 static password file
2 static token file
3 certificates
4 Identity services 3rd party


- Static Password file - basic, not recommended <Deprecated>
    -> .csv with rows of password, username, userId, groupId(optional)
    -> basic-auth-file
    -> `curl ... -u username:password`
- Static Token file    - basic, not recommended <Deprecated>
    -> .csv with rows of token, username, userId, groupId(optional)
    -> token-auth-file
    -> `curl ... --header "Authorization: Bearer <token>"`


