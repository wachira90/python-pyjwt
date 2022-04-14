# python jsonwebtoken pyjwt


```
!pip3 install pyjwt --user


Collecting pyjwt
  Using cached https://files.pythonhosted.org/packages/3f/32/d5d3cab27fee7f6b22d7cd7507547ae45d52e26030fa77d1f83d0526c6e5/PyJWT-2.1.0-py3-none-any.whl
Installing collected packages: pyjwt
Successfully installed pyjwt-2.1.0
```


```
import jwt
jwt.__version__

'2.1.0'
```

 

## encode

```
encoded = jwt.encode({"some": "payload"}, "secret", algorithm="HS256")
encoded

'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzb21lIjoicGF5bG9hZCJ9.Joh1R2dYzkRvDkqv3sygm5YyK8Gi4ShZqbhK2gxcs2U'

```


## decode


```
decode = jwt.decode(encoded, "secret", algorithms=["HS256"])
decode

{'some': 'payload'}
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


## with key file
```
>>> import jwt
>>> private_key = b"-----BEGIN PRIVATE KEY-----\nMIGEAgEAMBAGByqGSM49AgEGBS..."
>>> public_key = b"-----BEGIN PUBLIC KEY-----\nMHYwEAYHKoZIzj0CAQYFK4EEAC..."
>>> encoded = jwt.encode({"some": "payload"}, private_key, algorithm="RS256")
>>> print(encoded)
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzb21lIjoicGF5bG9hZCJ9.4twFt5NiznN84AWoo1d7KO1T_yoc0Z6XOpOVswacPZg
>>> decoded = jwt.decode(encoded, public_key, algorithms=["RS256"])
{'some': 'payload'}
```


## Specifying Additional Headers 
```
>>> jwt.encode(
...     {"some": "payload"},
...     "secret",
...     algorithm="HS256",
...     headers={"kid": "230498151c214b788dd97f22b85410a5"},
... )
'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjIzMDQ5ODE1MWMyMTRiNzg4ZGQ5N2YyMmI4NTQxMGE1In0.eyJzb21lIjoicGF5bG9hZCJ9.DogbDGmMHgA_bU05TAB-R6geQ2nMU2BRM-LnYEtefwg'
```

## Reading the Claimset without Validation
```
>>> jwt.decode(encoded, options={"verify_signature": False})
{'some': 'payload'}
```

## Reading the Claimset with Validation
```
>>> jwt.decode(encoded, options={"verify_signature": True})
{'some': 'payload'}
```

## Reading Headers without Validation
```
>>> jwt.get_unverified_header(encoded)
{'alg': 'RS256', 'typ': 'JWT', 'kid': 'key-id-12345...'}
```


## Expiration Time Claim (exp)
```
jwt.encode({"exp": 1371720939}, "secret")
jwt.encode({"exp": datetime.now(tz=timezone.utc)}, "secret")



try:
    jwt.decode("JWT_STRING", "secret", algorithms=["HS256"])
except jwt.ExpiredSignatureError:
    # Signature has expired
    ...
        
```

```
jwt_payload = jwt.encode(
    {"exp": datetime.datetime.now(tz=timezone.utc) + datetime.timedelta(seconds=30)},
    "secret",
)

time.sleep(32)

# JWT payload is now expired
# But with some leeway, it will still validate
jwt.decode(jwt_payload, "secret", leeway=10, algorithms=["HS256"])
```


```
jwt.decode(
    jwt_payload, "secret", leeway=datetime.timedelta(seconds=10), algorithms=["HS256"]
)

```






## Not Before Time Claim (nbf)
```
jwt.encode({"nbf": 1371720939}, "secret")
jwt.encode({"nbf": datetime.now(tz=timezone.utc)}, "secret")
```


## Issuer Claim (iss)
```
payload = {"some": "payload", "iss": "urn:foo"}

token = jwt.encode(payload, "secret")
decoded = jwt.decode(token, "secret", issuer="urn:foo", algorithms=["HS256"])
```


## Audience Claim (aud)
```
payload = {"some": "payload", "aud": ["urn:foo", "urn:bar"]}

token = jwt.encode(payload, "secret")
decoded = jwt.decode(token, "secret", audience="urn:foo", algorithms=["HS256"])
```


```
payload = {"some": "payload", "aud": "urn:foo"}

token = jwt.encode(payload, "secret")
decoded = jwt.decode(token, "secret", audience="urn:foo", algorithms=["HS256"])
```


```
payload = {"some": "payload", "aud": "urn:foo"}

token = jwt.encode(payload, "secret")
decoded = jwt.decode(
    token, "secret", audience=["urn:foo", "urn:bar"], algorithms=["HS256"]
)
```


## Issued At Claim (iat)
```
jwt.encode({"iat": 1371720939}, "secret")
jwt.encode({"iat": datetime.now(tz=timezone.utc)}, "secret")
```


## 
```
more info : https://pyjwt.readthedocs.io/en/stable/usage.html#encoding-decoding-tokens-with-hs256
```
