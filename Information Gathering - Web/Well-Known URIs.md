- .well-known standard, defined by RFC 8615, is a standardized directory within the website’s root domain
    
    - typically accessible through /.well-known/
    
    - simplifies discovery and access process for stakeholders, including web browsers, applications, and security tools
    
    - https://example.com/.well-known/security.txt
    
- IANA maintains a registry of .well-known URIs
|**URI Suffix**|**Description**|**Status**|**Reference**|
|---|---|---|---|
|`security.txt`|Contains contact information for security researchers to report vulnerabilities.|Permanent|RFC 9116|
|`/.well-known/change-password`|Provides a standard URL for directing users to a password change page.|Provisional|https://w3c.github.io/webappsec-change-password-url/\#the-change-password-well-known-uri|
|`openid-configuration`|Defines configuration details for OpenID Connect, an identity layer on top of the OAuth 2.0 protocol.|Permanent|http://openid.net/specs/openid-connect-discovery-1_0.html|
|`assetlinks.json`|Used for verifying ownership of digital assets (e.g., apps) associated with a domain.|Permanent|https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md|
|`mta-sts.txt`|Specifies the policy for SMTP MTA Strict Transport Security (MTA-STS) to enhance email security.|Permanent|RFC 8461|
- this is a small list of them, but these are notable ones
  
# Web Recon and .well-known
- openid-configuration
    
    - part of OpenID Connect Discovery
    
    - built on top of OAuth 2.0
    
```JavaScript
{
  "issuer": "https://example.com",
  "authorization_endpoint": "https://example.com/oauth2/authorize",
  "token_endpoint": "https://example.com/oauth2/token",
  "userinfo_endpoint": "https://example.com/oauth2/userinfo",
  "jwks_uri": "https://example.com/oauth2/jwks",
  "response_types_supported": ["code", "token", "id_token"],
  "subject_types_supported": ["public"],
  "id_token_signing_alg_values_supported": ["RS256"],
  "scopes_supported": ["openid", "profile", "email"]
}
```
- this gives multiple exploration opportunities
    
    - endpoint discovery
        
        - authorization endpoint
        
        - token endpoint
        
        - userinfo endpoint
        
    
    - JWKS URI
        
        - reveals the JSON Web Key Set (JWKS) - details the cryptographic keys used by server
        
    
    - supported scopes and reponse types
        
        - mapping out functionality and limitation of the OpenID Connect implementation
        
    
    - algorithm details
        
        - supported signing algorithms