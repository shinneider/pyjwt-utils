PyJwt Utils
=
PyJwt-Utils is a wrapper to facility your token encode/decode.

If you use or like the project, click `Star` and `Watch` to generate metrics and i evaluate project continuity.

# Install:
    pip install pyjwt-utils

# Usage:
1. In your file:
    ```
    from jwt_utils.django import JwtEncode, JwtDecode
    ...

    token = JwtEncode(
        signing_key='...',  # Default None 
        algorithm='HS256', # Default -> 'HS256', accepts ['HS256', 'HS384', 'HS512', 'RS256', 'RS384', 'RS512']
        iss=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.1)
        sub=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.2)
        aud=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.3)
        exp=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.4)
        nbf=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.5)
        iat=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.6)
        auto_iat=False,  # Default False (generate iat with current time) - (used only if IAT is None)
        jti=None,  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.7)
        auto_jti=False,  # Default False (generate jti with random hex) - (used only if JTI is None)
        from_time=None  # Default datetime.utcnow() - Base time from iss and nbf
        payload={}  # Your data
    ).encode()
    
    payload = JwtDecode(
        token='...'
        verify_iss=False,  # Default False (verify jwt iss)
        verify_aud=False,  # Default False (verify jwt aud)
        verify_exp=False,  # Default False (verify jwt exp)
        verify_nbf=False,  # Default False (verify jwt nbf)
        verify_iat=False,  # Default False (verify jwt iat)
        leeway=0  # Default 0 (validate time leeway)
    ).decode(
        raise_except=False  # Default False (check `jwt.exceptions` for available exceptions)
    )
    ```

# Usage (Django):

OBS: This require `Django` and `Django Rest Framework` to work.

1. Add to your `INSTALLED_APPS`, in `settings.py`:
    ```
    INSTALLED_APPS = [  
        ...
        'jwt_utils.django',
        ...
    ]
    ```

1. Add this configuration on `settings.py`:
    ```
    from datetime import timedelta
    ... 

    JWT_UTILS = {
        # all settings with None or False are ignored by default.

        # Generating Jwt
        'SIGNING_KEY': settings.SECRET_KEY,  # Default -> settings.SECRET_KEY
        'ALGORITHM': 'HS256',  # Default -> 'HS256', accepts ['HS256', 'HS384', 'HS512', 'RS256', 'RS384', 'RS512']
        'ISSUER': 'myapp',  # Default None -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.1)
        'SUBJECT': None,  # Default -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.2)
        'AUDIENCE': ['web', 'mobile'],  # Default -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.3)
        'EXPIRATION': timedelta(minutes=20),  # Default -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.4)
        'NOT_BEFORE': timedelta(seconds=0),  # Default -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.5)
        'IAT': None,  # Default -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.6)
        'AUTO_IAT': True, Auto generate IAT claim (used only if IAT is None)
        'JTI': True,  # Default -> None (https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.7)
        'AUTO_JTI': True, Auto generate JTI claim (used only if JTI is None)

        # Checking Jwt
        'VERIFY_ISSUER': True,  # Default `ISSUER is not None`
        'VERIFY_AUDIENCE': True,  # Default `AUDIENCE is not None`
        'VERIFY_EXPIRATION': True,  # Default `EXPIRATION is not None`
        'VERIFY_NOT_BEFORE': True,  # Default `NOT_BEFORE is not None`
        'VERIFY_IAT': True,  # Default `IAT is not None or AUTO_IAT`
        'VERIFY_MAX_LEEWAY': 0,  # Default 0, validate time leeway
    }
    ```

1. In your file:
    ```
    from jwt_utils.django import JwtEncode, JwtDecode
    ...

    token = JwtEncode(payload={}).encode()
    
    payload = JwtDecode(token).decode(
        raise_except=False  # Default False (check `jwt.exceptions` for available exceptions)
    )
    ```