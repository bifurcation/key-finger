---
title: Cryptographic Key Discovery with WebFinger
abbrev: KeyFinger
docname: draft-barnes-key-finger
date: 2014-07-24
category: exp

ipr: trust200902
area: General
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: R. Barnes
    name: Richard Barnes
    organization: Mozilla
    email: rlb@ipv.sx

normative:
  RFC2119:
  RFC7033:
  I-D.ietf-jose-json-web-key:
  I-D.ietf-jose-json-web-encryption:

informative:



--- abstract

TODO

--- middle

# Introduction

* The ability to discover keys is a major barrier to ubiquitous encryption
* Many protocols use user@domain identifiers -- email, xmpp, sip, webrtc
* WebFinger provides metadata for user@domain identifiers
* This doc defines metadata elements for keys

OPEN ISSUE: Properties are currently string or null.  We either need to update RFC 7033 to allow arbitrary JSON, or use a link relation (and accept the extra RTT).


# Public Keys

* Name = 'public_keys'
* Content = JWK Set
* Used for encrypting to and verifying signatures from the subject
* Can include certs in 'x5c' attribute 
* SHOULD encrypt to all 'enc' key
* SHOULD accept a sig from any 'sig' key


# Encrypted Private Keys

* Name = 'private_keys'
* Content = Encrypted JWK Set (JWE with JWK Set as payload)
* Used for provisioning clients
* For example, encrypt with PBKDF, decrypt with password


# IANA Considerations

## Registration of 'public_keys' property

## Registration of 'private_keys' property

# Security Considerations

* Authentication of user@domain identifiers
* Two authentication models:
  1. Cert-based: Query MAY be HTTP, MUST have 'x5c'
  2. HTTPS-based: Query MUST be HTTPS.  HTTPS auth proves domain authority; domain is trusted to assert keys.
* In other words, if a client performs a query over HTTP, keys without 'x5c' MUST be ignored
* In HTTPS-based model, domain holder is trusted authority.
  * Residual risk of domain holder asserting incorrect keys
  * Some legitimate cases for multple keys, e.g., logging or virus scanning
  * But need a way for users to know what keys have been asserted for them
  * Techniques such as CT could be applicable
