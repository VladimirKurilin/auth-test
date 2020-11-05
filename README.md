Dummy auth project
======================================

This is a mock implementation for oauth server using the User Login and Consent flow of [ory/hydra](https://www.ory.sh/docs/hydra/).

2 scope types are implemented:
- `openid` for an JWT ID token (username, email, sex and personal_info)
- `offline` for a refresh token


You can log in as any of the 10 predefined users:

| username   | email            | sex    |   password | very_useful_info       |
|:-----------|:-----------------|:-------|-----------:|:-----------------------|
| John       | john@kek.com     | male   |        123 | Hates cats             |
| Emily      | emily@kek.com    | female |        456 | Loves cats             |
| Jessica    | jessica@kek.com  | female |        789 |                        |
| Leonardo   | leonardo@kek.com | male   |        012 | Just a potato          |
| Alfred     | alfred@kek.com   | male   |        345 |                        |
| Juan       | juan@kek.com     | male   |        678 |                        |
| Sarah      | sarah@kek.com    | female |        901 |                        |
| Tony       | tony@kek.com     | male   |        234 | Stark. Mr Stark Junior |
| Megan      | megan@kek.com    |        |        567 | Modern society         |
| Robin      | robin@kek.com    | male   |        890 | Nolan forever!!        |


Running locally
---------------

Start dev environment with Docker:
```shell script
docker-compose up
```
Excellent! You a ready to go.


To check that everything works, you can run the example:

I. Register client application with id `auth-code-client` and secret `secret`.  If you need to create another client, you can regiser it in the same way ([full cli description here](https://www.ory.sh/hydra/docs/cli/hydra))
```shell script
docker-compose exec hydra \
    hydra clients create \
    --endpoint http://127.0.0.1:4445 \
    --id auth-code-client \
    --secret secret \
    --grant-types authorization_code,refresh_token \
    --response-types code,id_token \
    --scope openid,offline \
    --callbacks http://127.0.0.1:5555/callback
```

II. Start an example web application with Authorization Code Flow
```shell script
docker-compose exec hydra \
    hydra token user \
    --client-id auth-code-client \
    --client-secret secret \
    --endpoint http://127.0.0.1:4444/ \
    --port 5555 \
    --scope openid,offline
```