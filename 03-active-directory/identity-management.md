# Active Directory Identity Management

## User Account Model

The lab uses separate standard and privileged accounts for administrative personnel.

### Standard Account

Example:

`dreyes@corp.home.arpa`

Used for normal, non-privileged activities.

### Privileged Account

Example:

`adm-dreyes@corp.home.arpa`

Used for administrative activities.

## Administrative Account Naming

Privileged accounts use the `adm-` prefix to clearly distinguish them from standard user accounts.

## Least Privilege

Administrative privileges are not assigned simply because an account has an administrative naming convention.

Privileges are granted through security group membership and delegated permissions.

## Current Administrative OU

```text
_Admin
└── Admin-Accounts
