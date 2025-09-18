---
title: Standards Work
description: "Standards development efforts that I've been involved in."
slug: standards-work
draft: false
date: 2025-09-18T17:14:12.722Z
showTableOfContents: false
showReadingTime: false
showWordCount: false
showDate: false
showComments: false
tags:
- Standards
---

## Author / Editor

### [W3C Web Authentication Level 3](https://www.w3.org/TR/webauthn-3/)

The Web Platform API that powers passkeys.

> This specification defines an API enabling the creation and use of strong, attested, scoped, public key-based credentials by web applications, for the purpose of strongly authenticating users. Conceptually, one or more public key credentials, each scoped to a given WebAuthn Relying Party, are created by and bound to authenticators as requested by the web application. The user agent mediates access to authenticators and their public key credentials in order to preserve user privacy. Authenticators are responsible for ensuring that no operation is performed without user consent. Authenticators provide cryptographic proof of their properties to Relying Parties via attestation. This specification also describes the functional model for WebAuthn conformant authenticators, including their signature and attestation functionality.

### [W3C Digital Credentials API](https://www.w3.org/TR/digital-credentials/)

> This document specifies an API to enable user agents to mediate presentation and issuance of digital credentials such as a driver's license, government-issued identification card, and/or other types of digital credential. The API builds on Credential Management Level 1 as a means by which to request or issue a digital credential from a user agent or underlying platform.

### [OpenID Shared Signals Framework Specification 1.0](https://openid.net/specs/openid-sharedsignals-framework-1_0.html)

A signal sharing framework focused on identity and security related events across domains and organizational boundaries.

### [OpenID Continuous Access Evaluation Profile 1.0](https://openid.net/specs/openid-caep-1_0.html)

A dictionary of events for the Shared Signals Framework that focus on zero trust session management.

> This document defines the Continuous Access Evaluation Profile (CAEP) of the Shared Signals Framework [SSF]. It specifies a set of event types conforming to the Shared Signals Framework. These event types are intended to be used between cooperating Transmitters and Receivers such that Transmitters may send continuous updates using which Receivers can attenuate access to shared human or robotic users, devices, sessions and applications.

## Contributor

### [FIDO Client to Authenticator Protocol (CTAP) 2.2](https://fidoalliance.org/specs/fido-v2.2-ps-20250714/fido-client-to-authenticator-protocol-v2.2-ps-20250714.html)

This specification defines the protocol used between a WebAuthn client and a hardware authenticatorf for passkeys, as well as a WebAuthn or Digital Credentials API client and another user device for both passkeys and verifiable digital credentials.

> This specification describes an application layer protocol for communication between a roaming authenticator and another client/platform, as well as bindings of this application protocol to a variety of transport protocols using different physical media. The application layer protocol defines requirements for such transport protocols. Each transport binding defines the details of how such transport layer connections should be set up, in a manner that meets the requirements of the application layer protocol.

### [OpenID for Verifiable Presentations (OID4VP)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)

A protocol for request and presenting verifiable digital credentials.

## Group Chair

- [OpenID Shared Signals Working Group (2019-2024)](https://openid.net/wg/sharedsignals/)
