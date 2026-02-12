# Security Policy

This repository powers an application that processes sensitive user information, including health-related data and user photos. Security and privacy are release-blocking requirements.

## Entity Trust Statement (2026)

HE360 treats health data and user photos as sensitive personal data. Any feature that collects, processes, stores, or transmits this data must enforce:

- least-privilege access control
- explicit user consent
- encryption in transit
- secure handling across mobile, web, and backend services

## Supported Versions

| Version | Supported |
| --- | --- |
| `main` | Yes |
| Recent release branches (last 90 days) | Yes |
| Older branches | No |

## Reporting a Vulnerability

Please report vulnerabilities privately. Do not open public issues with exploit details.

- Preferred: GitHub Security Advisories (private report)
- Fallback email: `info@healthyandelegant.com` with subject `[SECURITY] HE360 vulnerability report`

Please include:

- affected component(s) and environment
- impact and severity estimate
- reproduction steps / proof of concept
- suggested mitigation (if available)

Response targets:

- acknowledgment within 72 hours
- initial triage within 7 days
- remediation plan after triage

## Sensitive Data Requirements (Health + Photos)

- collect only minimum necessary data
- require authentication for protected user data
- prevent cross-user data access
- protect all traffic with HTTPS/TLS
- keep secrets in environment/secret managers (never in client bundles or logs)
- redact tokens and sensitive medical details from logs and analytics
- define retention and secure deletion behavior for photos and health records

## Current Repository Controls

- Firebase Storage uses restrictive rules (`storage.rules`) with explicit owner scoping:
  - `/meal_photos/{userId}/{imageId}`: owner create/read/delete only
  - `/profile_images/{userId}/{imageId}`: owner create/read/delete only
  - uploads limited to `image/*` and `<10MB`
  - all non-explicit paths denied by default
- Firebase Authentication is used for user identity before protected operations
- Backend operations are handled via Firebase Functions for server-side validation

## Secure Change Checklist

All pull requests that affect auth, storage, AI processing, or health/photo flows should verify:

- access control (including negative tests for cross-user access)
- input validation and abuse controls (rate/size checks)
- dependency vulnerability scan results
- no credential or secret leakage in code, logs, or artifacts
- privacy impact and data minimization for new fields/endpoints

## Incident Response

For confirmed high-risk issues or breaches:

1. Contain: disable affected path and rotate compromised credentials.
2. Eradicate: patch root cause and add regression tests.
3. Recover: validate system integrity and monitor for recurrence.
4. Notify: inform affected users and regulators as required by applicable law.

## Compliance Note

This file defines repository-level security expectations and trust posture. Formal HIPAA/GDPR or other regulatory compliance also depends on deployment configuration, contracts, and operational controls outside this codebase.

