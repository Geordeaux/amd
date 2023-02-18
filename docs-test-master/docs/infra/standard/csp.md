---
id: csp
title: Content Security Policy
---

## Content Security Policy (CSP)
To ensure only authorised script and external sources (media, images and css) are allowed on our webpages.

## How to achieve CSP?
We ensure that all sources that are whitelisted are listed in our CSP. All CSP rules will be in the response header of the page.

Other than that, a unique `nonce` will be generated for each page for inline JavaScript. 

## Resources
- https://content-security-policy.com/ or https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
- https://csp-evaluator.withgoogle.com/

## Full diagram
import Mermaid from '@theme/Mermaid'

<Mermaid chart={`
sequenceDiagram
    participant Browser
    participant CDN
    participant Server
    participant Redis
    participant CSP
    participant Shuvi
    Browser->>CDN: request page
    alt cache found
      CDN->>Browser: return cached version
    else cache mismatch
      CDN->>Server: request page 
    end
    Server->>CSP: setup CSP
    alt nonce-enabled
      CSP->>Server: inject nonce placeholder
    else nonce-disabled
      CSP->>Server: inject unsafe-inline 
    end
    Server->>Redis: check for cahced version
    alt redis found
      Redis->>Server: cache found
    else not found
      Redis->>Server: not found
      Server->>Shuvi: render page
      Shuvi->>Server: return html
      Server->>Redis: save result
    end
    alt nonce-enabled
      CSP->>Server: replace nonce placeholder with uuid
    end
    Server->>CDN: return html
    CDN->>Browser: save html cache and return
    Browser->>Browser: check response cache-control, etag
`}>
</Mermaid>

## Future consideration
- Replace current flow with edge lambda to inject `nonce`.
