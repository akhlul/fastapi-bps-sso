# SSO BPS OpenID Connect for FastAPI

`fastapi-bps-sso` is an extension to [FastAPI](https://fastapi.tiangolo.com/)
that allows you to add OpenID Connect based authentication for your endpoints
within minutes.

It is just a fork to make it easier for me to maintain.

## Installation

```
pdm add pyjwt cryptography
pdm add "git+https://github.com/akhlul/fastapi-bps-sso.git"
```

## How to use

The package provides a simple decorator `@oidc.login_required` to protect
an endpoint. You can retrieve the user info directly from the request object.

```py
from typing import Dict

from fastapi import FastAPI
from fastapi import Request

from fastapi_oidc_auth.auth import OpenIDConnect

# realm (e.g. Keycloak instance)
host = "https://sso.bps.go.id"
realm = "example-realm"
client_id = "example-client"
client_secret = "xxx765cd-20ba-44a3-9584-784807a36906"
app_uri = "http://localhost:5000"

oidc = OpenIDConnect(host, realm, app_uri, client_id, client_secret)
app = FastAPI()


@app.get("/very-secret")
@oidc.require_login
async def very_secret(request: Request) -> Dict:
    return {"message": "success", "user_info": request.user_info}
```

## Notes

- It won't work if you access the app at `http://localhost:5000`, but the app_uri config is set to `http://127.0.0.1:5000`. Both URLs point to the same website you're developing, but the mismatch will cause issues.
