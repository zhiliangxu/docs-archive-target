---
title: Security Guidelines
description: Rules for developing secure services for APEX
---
# Security Guidelines

All projects must ensure full compliance with all Azure security guidelines. Their rules always supersede anything in these documents.

## All secrets in Key Vault

No secrets (api keys, logins, etc.) should ever be stored on developer machines or checked in to source code. All secrets must be stored in Azure Key Vault and retrieved at run time via AAD authentication. AAD credentials may be stored on a developer's machine using the User Secrets library. Each developer must create their own set of credentials and not share them with anyone else. At run time, the app will retrieve its own set of credentials to access the Key Vault.

## Geneva monitoring on all services

Geneva provides both monitoring of machines and the Azure Security Pack which protects Microsoft assets from intrusion. On-boarding to this system is not simple and beyond the scope of this document. Work with the SRE team to ensure your service is compliant here.