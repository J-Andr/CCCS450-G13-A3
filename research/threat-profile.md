# Threat profile and scope

## Selected threat

**Scattered Spider** (MITRE ATT&CK group **G1015**).

## Fictional organization and assumptions

Northstar Community Health is a fictional regional healthcare provider. Its critical asset is the identity service that grants staff access to a patient-appointment application and administrative systems. The assumed environment has a cloud single sign-on (SSO) tenant connected to a Windows-based directory, a small Linux server fleet, and a help desk able to reset passwords and MFA enrollment. These are scenario assumptions, not facts about a real organization or a claim that Scattered Spider targeted this fictional entity. The lab test is separate from the threat research and must run only on an isolated, group-owned, non-production VM with fictitious accounts and no real patient or personal data.

## Why this threat fits

The CISA joint advisory describes Scattered Spider's identity-centered activity, including social engineering of help desks to reset passwords or transfer MFA tokens, account takeovers in SSO environments, MFA-token registration, and historical federation-trust changes. These behaviours make identity and access controls a relevant focus for the fictional organization. The advisory also describes account and directory discovery, allowing the research to span Discovery as well as access, persistence, and privilege-related tactics.

## Scope and evidence boundary

The four mappings in [the mapping table](attack-mapping.md) are reported behaviours attributed to Scattered Spider. The proposed local process-discovery test is **lab-only** and is not one of those four mappings. A local `ps` execution can validate that the lab's host-audit source recorded a process launch; it cannot establish that Scattered Spider used that command, that any threat activity occurred, or that a real identity system is protected.