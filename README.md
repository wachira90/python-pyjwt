# python jsonwebtoken pyjwt


```python
!pip3 install pyjwt --user
```

    Collecting pyjwt
      Using cached https://files.pythonhosted.org/packages/3f/32/d5d3cab27fee7f6b22d7cd7507547ae45d52e26030fa77d1f83d0526c6e5/PyJWT-2.1.0-py3-none-any.whl
    Installing collected packages: pyjwt
    Successfully installed pyjwt-2.1.0



```python
import jwt
jwt.__version__
```




    '2.1.0'



## encode


```python
encoded = jwt.encode({"some": "payload"}, "secret", algorithm="HS256")
encoded
```




    'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzb21lIjoicGF5bG9hZCJ9.Joh1R2dYzkRvDkqv3sygm5YyK8Gi4ShZqbhK2gxcs2U'



## decode


```python
decode = jwt.decode(encoded, "secret", algorithms=["HS256"])
decode
```




    {'some': 'payload'}




```python

```

```
try:
    jwt.decode("JWT_STRING", "secret", algorithms=["HS256"])
except jwt.ExpiredSignatureError:
    # Signature has expired
    ...
```

```
jwt_payload = jwt.encode(
    {"exp": datetime.datetime.utcnow() + datetime.timedelta(seconds=30)}, "secret"
)

time.sleep(32)

# JWT payload is now expired
# But with some leeway, it will still validate
jwt.decode(jwt_payload, "secret", leeway=10, algorithms=["HS256"])

------------------------

jwt.decode(
    jwt_payload, "secret", leeway=datetime.timedelta(seconds=10), algorithms=["HS256"]
)

```

## issuser

```
payload = {"some": "payload", "iss": "urn:foo"}

token = jwt.encode(payload, "secret")
decoded = jwt.decode(token, "secret", issuer="urn:foo", algorithms=["HS256"])
```

# Retrieve RSA signing keys from a JWKS endpoint

```
>>> import jwt
>>> from jwt import PyJWKClient
>>> token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik5FRTFRVVJCT1RNNE16STVSa0ZETlRZeE9UVTFNRGcyT0Rnd1EwVXpNVGsxUWpZeVJrUkZRdyJ9.eyJpc3MiOiJodHRwczovL2Rldi04N2V2eDlydS5hdXRoMC5jb20vIiwic3ViIjoiYVc0Q2NhNzl4UmVMV1V6MGFFMkg2a0QwTzNjWEJWdENAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vZXhwZW5zZXMtYXBpIiwiaWF0IjoxNTcyMDA2OTU0LCJleHAiOjE1NzIwMDY5NjQsImF6cCI6ImFXNENjYTc5eFJlTFdVejBhRTJINmtEME8zY1hCVnRDIiwiZ3R5IjoiY2xpZW50LWNyZWRlbnRpYWxzIn0.PUxE7xn52aTCohGiWoSdMBZGiYAHwE5FYie0Y1qUT68IHSTXwXVd6hn02HTah6epvHHVKA2FqcFZ4GGv5VTHEvYpeggiiZMgbxFrmTEY0csL6VNkX1eaJGcuehwQCRBKRLL3zKmA5IKGy5GeUnIbpPHLHDxr-GXvgFzsdsyWlVQvPX2xjeaQ217r2PtxDeqjlf66UYl6oY6AqNS8DH3iryCvIfCcybRZkc_hdy-6ZMoKT6Piijvk_aXdm7-QQqKJFHLuEqrVSOuBqqiNfVrG27QzAPuPOxvfXTVLXL2jek5meH6n-VWgrBdoMFH93QEszEDowDAEhQPHVs0xj7SIzA"
>>> kid = "NEE1QURBOTM4MzI5RkFDNTYxOTU1MDg2ODgwQ0UzMTk1QjYyRkRFQw"
>>> url = "https://dev-87evx9ru.auth0.com/.well-known/jwks.json"
>>> jwks_client = PyJWKClient(url)
>>> signing_key = jwks_client.get_signing_key_from_jwt(token)
>>> data = jwt.decode(
...     token,
...     signing_key.key,
...     algorithms=["RS256"],
...     audience="https://expenses-api",
...     options={"verify_exp": False},
... )
>>> print(data)
{'iss': 'https://dev-87evx9ru.auth0.com/', 'sub': 'aW4Cca79xReLWUz0aE2H6kD0O3cXBVtC@clients', 'aud': 'https://expenses-api', 'iat': 1572006954, 'exp': 1572006964, 'azp': 'aW4Cca79xReLWUz0aE2H6kD0O3cXBVtC', 'gty': 'client-credentials'}
```


```python

```


```python

```


```python

```
