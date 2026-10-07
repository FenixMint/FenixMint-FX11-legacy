# FX11 data minimization, redaction and public-artifact policy

Status: **OWNER DECISION / NON-NEGOTIABLE**

FX11 Legacy must not leak private, account-specific or uniquely identifying machine data into source control, public issues, test fixtures, screenshots, example logs or generated reports intended for sharing.

This repository is public. Treat every committed artifact as public information.

## Core rule

**Collect only what is required for the technical decision. Persist even less. Export only sanitized data.**

Hardware-aware does **not** mean identity-aware.

FX11 needs to know what class/model of hardware exists; it usually does not need to know the unique identity of the individual device or user.

## Never commit or publish

Do not place any of the following in Git, GitHub issues, docs, fixtures, screenshots, example output or shared diagnostics:

- hardware serial numbers;
- chassis serial numbers;
- motherboard serial numbers;
- SSD/NVMe/HDD serial numbers;
- monitor serial numbers;
- battery serial numbers;
- Windows product keys;
- activation tokens or secrets;
- Microsoft account email addresses;
- Office/M365 account addresses;
- OneDrive account/tenant identifiers;
- local usernames;
- real user profile names;
- computer/host names when they identify a real deployment;
- domain names or Active Directory / Entra tenant identifiers belonging to a real deployment;
- MAC addresses;
- public IP addresses;
- private/local IP addresses unless a synthetic example is used;
- Wi-Fi SSIDs/BSSIDs;
- VPN endpoint/account details;
- device-specific certificates or thumbprints when tied to a real deployment;
- unique device GUIDs/UUIDs/installation identifiers when not strictly required;
- TPM endorsement/identity material;
- BitLocker recovery keys;
- browser/session tokens;
- API tokens/credentials;
- exact user document paths;
- file names or recent-file history that can reveal user/business data;
- telemetry/event-log message content containing user/account/device identifiers;
- geolocation/location history;
- clipboard history;
- recent document lists;
- application data containing customer/business/private content.

If a field is not necessary for a rule decision, do not collect it.

## Allowed hardware identity

Examples of information that is normally safe and useful:

- manufacturer: Lenovo / HP / Dell / ASUS / ASRock;
- model family/product: ThinkPad L13 Gen 2 AMD, ASRock LiveMixer B650;
- CPU model: Ryzen 5 PRO 5650U;
- GPU model: GeForce RTX 4060;
- storage model/capacity without serial number;
- RAM capacity/type/speed;
- BIOS vendor and version, but not machine serial;
- baseboard product/model, but not unique serial;
- Windows edition/build;
- power capability such as Modern Standby/S3;
- TPM presence/version without unique identity material;
- Secure Boot enabled/disabled;
- installed package/service/task names;
- driver provider/version/device class;
- sensor class/name/value when it does not contain a unique serial/account identifier.

## Local raw discovery vs exported report

FX11 may need to query Windows APIs that return both useful and sensitive fields.

Preferred behavior:

1. query only the properties needed;
2. never request sensitive properties if a narrower query exists;
3. if a Windows API returns sensitive fields anyway, discard them immediately;
4. keep raw sensitive values out of persistent state;
5. write sanitized objects to audit/checkpoint output;
6. apply a final redaction pass before any export/share operation.

### Example

Bad:

```json
{
  "UserName": "REALDOMAIN\\realuser",
  "ComputerName": "REAL-PC-15",
  "BoardSerial": "ABC123456",
  "MacAddress": "00-11-22-33-44-55"
}
```

Good:

```json
{
  "SystemRole": "office-workstation",
  "SystemManufacturer": "HP",
  "SystemModel": "Example model",
  "BaseBoardManufacturer": "ASRock",
  "BaseBoardProduct": "LiveMixer B650",
  "NetworkAdapterModel": "Intel Ethernet Controller"
}
```

## Paths and usernames

Never serialize raw user-profile paths such as:

`C:\\Users\\realname\\...`

Normalize paths where technically useful:

```
%USERPROFILE%\...
%PROGRAMDATA%\...
%WINDIR%\...
%PROGRAMFILES%\...
```

When a path cannot be safely normalized and is not essential, omit it.

## Hostname policy

Real deployment hostnames should not be used in committed examples.

Use synthetic names such as:

- `FX11-TEST-01`
- `LAB-HP-01`
- `LAB-ASROCK-01`

Do not reuse real office/laptop hostnames in docs or fixtures.

## Network policy

Audit network **capabilities and adapter models**, not network identity.

Useful:

- Ethernet/Wi-Fi present;
- adapter vendor/model;
- driver version;
- link capability;
- power-management capability.

Not useful for normal FX11 policy:

- MAC;
- current IP;
- gateway;
- DNS search suffix belonging to an organization;
- SSID;
- public IP.

Do not collect those fields unless a future dedicated network diagnostic explicitly requires them, and then keep them out of public artifacts.

## User/account policy

FX11 hardware/system audit does not need the real user identity.

Prefer:

- account type / local vs Microsoft vs domain/Entra as a category;
- privilege class (admin/standard);
- presence of OneDrive/Office/Teams as capability/state.

Do not export:

- username;
- email;
- tenant;
- SID when tied to a real user;
- account object IDs.

## Event logs

Event logs frequently contain:

- usernames;
- hostnames;
- paths;
- device IDs;
- application data.

Do not dump raw event logs into GitHub.

If an event is needed as a fixture or bug report:

1. extract only the minimal relevant fields;
2. redact identity-bearing values;
3. use synthetic placeholders;
4. preserve event ID/provider/meaning, not private payload.

## Screenshots

Before committing or sharing a screenshot, inspect for:

- user/account name;
- email/avatar;
- computer name;
- serial/service tag;
- local/network path;
- Wi-Fi/network name;
- browser tabs/history;
- OneDrive/Teams tenant information;
- customer/business data.

Prefer generated/synthetic screenshots for documentation.

## Checkpoints and audit files

Default checkpoint/audit format must be safe to store locally and safe to share accidentally.

Therefore:

- sensitive fields are excluded by schema;
- redaction is not only a UI/display layer;
- raw secrets/identifiers must not exist in the normal serialized audit object;
- tests must assert that forbidden fields do not appear.

A future explicit `--diagnostic-private` mode, if ever introduced, must be clearly separated, local-only, opt-in and never used by default. It is not part of the current baseline.

## Pseudonymous identifiers

Prefer no identifier when one is not needed.

If the application needs to correlate records inside one audit run, use a random run-scoped identifier.

Do not derive a stable public ID directly from serial/MAC/username. Unsalted hashes of unique hardware/user identifiers are still identifying.

Any future persistent pseudonymous installation ID must be:

- random;
- unrelated to hardware/account identifiers;
- kept local by default;
- excluded from public/shared export unless explicitly needed.

## Test fixtures

Tests must use synthetic data only.

Examples may use:

- `LAB-HP-01`;
- `LAB-ASROCK-01`;
- fake serial `TEST-SERIAL-0001` only when testing redaction itself;
- documentation-reserved IPs such as `192.0.2.0/24`, `198.51.100.0/24`, `203.0.113.0/24`;
- synthetic usernames such as `fx11-test-user`.

Never copy a real machine audit and merely rename the file.

## Git preflight

Before commit or issue upload, review generated artifacts for private data.

Future tooling should provide an automated **privacy preflight** capable of detecting likely:

- serial numbers;
- email addresses;
- MAC addresses;
- IPv4/IPv6 addresses;
- Windows user profile paths;
- account SIDs;
- product keys/recovery keys;
- known real hostnames supplied only in local ignore configuration.

Privacy preflight is defense in depth, not permission to collect unnecessary data.

## Architecture requirement

All Discovery/Audit modules must distinguish between:

- **decision facts** — safe facts required by FX11 policy;
- **private identity facts** — forbidden from normal persisted/exported state.

The planner consumes decision facts only.

## Repository rule

No real private identifier may be added to this repository, even if it was previously shown in a development conversation or terminal output.

If such data is discovered in Git history:

1. stop further copying;
2. identify scope;
3. remove/redact it from current files;
4. evaluate whether Git history rewrite / credential or identifier rotation is required;
5. document the remediation without repeating the private value.

## Relationship to FX11 goals

Privacy is not only about reducing Microsoft telemetry.

FX11 must also protect the user's privacy in **its own diagnostics, logs, checkpoints, test fixtures and support workflow**.
