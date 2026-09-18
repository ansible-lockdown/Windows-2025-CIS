# Windows Server 2025 CIS

## Configure a Microsoft Server 2025 machine to be [CIS](https://www.cisecurity.org/cis-benchmarks/) compliant

### Based on [ CIS Microsoft Windows Server 2025 v1.0.0 - 03-19-2025 ](https://www.cisecurity.org/cis-benchmarks/)

---

## Public Repository

![Org Stars](https://img.shields.io/github/stars/ansible-lockdown?label=Org%20Stars&style=social)
![Stars](https://img.shields.io/github/stars/ansible-lockdown/Windows-2025-CIS?label=Repo%20Stars&style=social)
![Forks](https://img.shields.io/github/forks/ansible-lockdown/Windows-2025-CIS?style=social)
![Followers](https://img.shields.io/github/followers/ansible-lockdown?style=social)
[![X URL](https://img.shields.io/twitter/url/https/x.com/AnsibleLockdown.svg?style=social&label=Follow%20%40AnsibleLockdown)](https://x.com/AnsibleLockdown)
![Discord Badge](https://img.shields.io/discord/925818806838919229?logo=discord)

![License](https://img.shields.io/github/license/ansible-lockdown/Windows-2025-CIS?label=License)

## Lint & Pre-Commit Tools

[![Pre-Commit.ci](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Windows-2025-CIS/pre-commit-ci.json)](https://results.pre-commit.ci/latest/github/ansible-lockdown/Windows-2025-CIS/devel)
![YamlLint](https://img.shields.io/badge/yamllint-Present-brightgreen?style=flat&logo=yaml&logoColor=white)
![Ansible-Lint](https://img.shields.io/badge/ansible--lint-Present-brightgreen?style=flat&logo=ansible&logoColor=white)

## Community Release Information

![Release Branch](https://img.shields.io/badge/Release%20Branch-Main-brightgreen)
![Release Tag](https://img.shields.io/github/v/tag/ansible-lockdown/Windows-2025-CIS?label=Release%20Tag&&color=success)
![Main Release Date](https://img.shields.io/github/release-date/ansible-lockdown/Windows-2025-CIS?label=Release%20Date)
![Benchmark Version Main](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Windows-2025-CIS/benchmark-version-main.json)
![Benchmark Version Devel](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Windows-2025-CIS/benchmark-version-devel.json)

[![Main Pipeline Status](https://github.com/ansible-lockdown/Windows-2025-CIS/actions/workflows/main_pipeline_validation.yml/badge.svg?)](https://github.com/ansible-lockdown/Windows-2025-CIS/actions/workflows/main_pipeline_validation.yml)
[![Devel Pipeline Status](https://github.com/ansible-lockdown/Windows-2025-CIS/actions/workflows/devel_pipeline_validation.yml/badge.svg?)](https://github.com/ansible-lockdown/Windows-2025-CIS/actions/workflows/devel_pipeline_validation.yml)

![Devel Commits](https://img.shields.io/github/commit-activity/m/ansible-lockdown/Windows-2025-CIS/devel?color=dark%20green&label=Devel%20Branch%20Commits)
![Open Issues](https://img.shields.io/github/issues-raw/ansible-lockdown/Windows-2025-CIS?label=Open%20Issues)
![Closed Issues](https://img.shields.io/github/issues-closed-raw/ansible-lockdown/Windows-2025-CIS?label=Closed%20Issues&&color=success)
![Pull Requests](https://img.shields.io/github/issues-pr/ansible-lockdown/Windows-2025-CIS?label=Pull%20Requests)

---

## Subscriber Release Information

![Private Release Branch](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Private-Windows-2025-CIS/release-branch.json)
![Private Benchmark Version](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Private-Windows-2025-CIS/benchmark-version.json)

[![Private Remediate Pipeline](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Private-Windows-2025-CIS/remediate.json)](https://github.com/ansible-lockdown/Private-Windows-2025-CIS/actions/workflows/main_pipeline_validation.yml)

![Private Pull Requests](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Private-Windows-2025-CIS/prs.json)
![Private Closed Issues](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_windows_IaC/badges/Private-Windows-2025-CIS/issues-closed.json)

---

## Looking for support?

[Lockdown Enterprise](https://www.lockdownenterprise.com#GH_AL_WINDOWS_2025_cis)

[Ansible support](https://www.mindpointgroup.com/cybersecurity-products/ansible-counselor#GH_AL_WINDOWS_2025_cis)

### Community

On our [Discord Server](https://www.lockdownenterprise.com/discord) to ask questions, discuss features, or just chat with other Ansible-Lockdown users

### Contributing

Bug reports and feature requests are welcome from everyone, please raise an issue.

Pull requests are accepted from approved contributors only. To be onboarded, join the [Discord Server](https://www.lockdownenterprise.com/discord) and request contributor access. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full process.

---

## Caution(s)

This role **will make changes to the system** which may have unintended consequences. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

Check Mode is not supported! The role will complete in check mode without errors, but it is not supported and should be used with caution.

This role was developed against a clean install of the Windows 2025 Operating System. If you are implementing to an existing system please review this role for any site specific changes that are needed.

To use release version please point to main branch and relevant release for the cis benchmark you wish to work with.

---

## Domain Members

Account Policy is domain scoped. On a domain joined host the Default Domain Policy
overwrites local `[System Access]` at every Group Policy refresh, so section 1 cannot
hold there. That is CIS's own position - on a member server, Account Policies belong
in the Default Domain Policy.

Measured on a member server after joining the domain:

| setting | applied by the role | after the domain refresh |
|---|---|---|
| MinimumPasswordLength | 14 | 7 |
| MaximumPasswordAge | 365 | 42 |
| LockoutBadCount | 5 | 0 (`net accounts`: Never) |
| LockoutDuration | 15 | removed |
| ResetLockoutCount | 15 | removed |

All 11 section 1 controls revert. Once `LockoutBadCount` is 0 Windows removes
`LockoutDuration` and `ResetLockoutCount` outright, which used to abort the play on a
second run. The role now detects a domain joined host and skips section 1 with a
warning instead.

Only the secedit backed controls are affected - registry backed controls hold. Set
section 1 from a GPO on domain members.

### Controls the domain overrides

On any host that is not standalone - a member server or a domain controller - these
are set by domain policy, so the role does not apply them and the audit reports them
as skipped, with the reason in `meta.skip_reason`. Both decide from the host itself:
the role from its prelim facts, the audit from `run_audit.ps1`, which reads
`Win32_ComputerSystem.DomainRole` and passes `win25cis_system_role` inline.

| Control | Setting | Why the domain wins | Role | Audit |
|---|---|---|---|---|
| 1.1.1 - 1.1.5, 1.1.7 | Password policy | Default Domain Policy sets `[System Access]` | Skipped, with a warning | Skipped, with the reason |
| 1.2.1 - 1.2.4 | Account lockout policy (1.2.3 member server only) | Default Domain Policy sets `[System Access]` | Skipped, with a warning | Skipped, with the reason |
| 2.3.11.6 | Force logoff when logon hours expire | Default Domain Policy sets `ForceLogoffWhenHourExpire = 0` | Skipped, with a warning | Skipped, with the reason |
| 2.3.5.4 | LDAP server signing (domain controller only) | Default Domain Controllers Policy sets `LDAPServerIntegrity = 1`, reverted at the next refresh | Applied, with a warning | Expected to fail on a DC |
| 1.1.6 | Relax minimum password length limits (member server only) | Not overridden - a registry value, not account policy | Applied | Asserted |

Set the skipped controls in the Default Domain Policy, and 2.3.5.4 in the Default
Domain Controllers Policy. On a standalone server every one of them is applied and
asserted as normal.

#### Tags describe the CIS profile, not where the task runs

A control's `level1-memberserver` / `level1-domaincontroller` tags come straight from
its Profile Applicability in the benchmark, and they never change with the host. The
`when` decides whether the task can do anything on this host. The two can disagree,
and that is deliberate.

1.2.3 is the clearest case. CIS scopes it to Level 1 Member Server, so it carries
`level1-memberserver` and not `level1-domaincontroller`. It is also an
`AllowAdministratorLockout` value in `[System Access]`, so it only holds on a
standalone server, and its `when` restricts it to one. A domain member server -
exactly the profile the tag names - therefore skips it and takes it from the Default
Domain Policy. Retagging it to match the `when` would misreport the benchmark, and
loosening the `when` to match the tag would write a value the domain reverts.

### What the role enforces on each target type

The benchmark has 462 recommendations. The role holds a task for every one of them,
but CIS scopes many to a single profile, and Windows scopes Account Policy to the
domain. Counting only the host facts the prelim sets - not the per rule toggles - a
default run enforces:

| Target | Enforced by this role | Skipped: not in the CIS profile for this target | Skipped: externally managed |
|---|---|---|---|
| Standalone server | 395 | 67 | 0 |
| Domain member server | 420 | 31 | 11 |
| Domain controller | 410 | 42 | 10 |

"Not in the CIS profile" means the benchmark itself does not apply the control to
that target - the 31 skipped on a member server are the Level 1 Domain Controller
controls, and the DC skips the Member Server ones. Those are not gaps.

"Externally managed" is the set the role cannot make stick locally. On a member
server that is 1.1.1 - 1.1.5, 1.1.7, 1.2.1 - 1.2.4 and 2.3.11.6. A domain controller
is the same list less 1.2.3, which CIS scopes to member servers only. Every one of
these is a `[System Access]` value: a domain controller has no local account database
for domain accounts, and on a member server the Default Domain Policy overwrites the
local value at the next Group Policy refresh. A local `secedit` write is not a
partial fix on either - it is a no-op that reports `changed`.

There is one remediation path for these, and it is the Default Domain Policy. The
`Windows-2025-CIS-GPO` role authors it - see [Group Policy Objects](#group-policy-objects).
A clean run of this role against a domain controller is not evidence that those ten
controls are enforced; it reports them as skipped, with the reason, and the audit
does the same.

---

## Matching A Security Level For CIS

It is possible to only run level 1 or level 2 controls for CIS as well as other profile definitions that are set in the CIS release.
This is managed using tags:

- level1-domaincontroller
- level1-memberserver
- level2-domaincontroller
- level2-memberserver
- level1-domainmember
- ngws-domaincontroller
- ngws-memberserver

The control found in defaults main also need to reflect this as this control the testing that takes place if you are using the audit component.

## Coming From A Previous Release

CIS release always contains changes, it is highly recommended to review the new references and available variables. This have changed significantly since ansible-lockdown initial release.
This is now compatible with python3 if it is found to be the default interpreter. This does come with pre-requisites which it configures the system accordingly.

Further details can be seen in the [Changelog](./ChangeLog.md)

## Group Policy Objects

This role applies the benchmark directly to the host it runs against. It does not
create Group Policy Objects. Creating CIS GPOs on a domain controller is provided
separately to subscribers by the `Windows-2025-CIS-GPO` role.

### Migrating from the in-role GPO path

GPO creation used to live in this role behind `win25cis_create_gpos`. It now lives in
`Windows-2025-CIS-GPO` (`mindpointgroup.windows2025_cis_gpo`), which runs against a
domain controller and writes the CIS GPOs, including the Default Domain Policy that
carries Account Policy.

The variables kept their names. Move them from this role's inventory to the GPO
role's and they behave as before:

| Removed from this role | Now set on | Notes |
|---|---|---|
| `win25cis_create_gpos` | - | Dropped. The GPO role only creates GPOs, so there is nothing to switch on. |
| `win25cis_ansible_remediation` | - | Dropped. This role only remediates, so there is nothing to switch off. |
| `win25cis_create_domain`, `win25cis_domain`, `win25cis_safe_mode_administrator_password` | `Windows-2025-CIS-GPO` | Test domain promotion. Unchanged names and defaults. Set the DSRM password from a vault, never in the playbook. |
| `win25cis_default_domain_policy_gpo`, `win25cis_l1_dc_gpo`, `win25cis_l1_ms_gpo`, `win25cis_l2_dc_gpo`, `win25cis_l2_ms_gpo`, `win25cis_l1_dm_gpo`, `win25cis_services_dc_gpo`, `win25cis_services_ms_gpo`, `win25cis_ngws_dc_gpo`, `win25cis_ngws_ms_gpo`, `win25cis_l1_user_gpo`, `win25cis_l2_user_gpo` | `Windows-2025-CIS-GPO` | Which GPOs to create. Unchanged names and defaults. |
| The matching `*_gpo_name` variables, `win25cis_gpo_hardening_version`, `win25cis_gpo_hardening_os` | `Windows-2025-CIS-GPO` | GPO naming. Unchanged. |
| `win25cis_copy_policy_definitions`, `win25cis_copy_cis_custom_admx_adml` | `Windows-2025-CIS-GPO` | ADMX/ADML staging. Unchanged. |
| `win25cis_backup_custom_gpos`, `win25cis_backup_location_style` | `Windows-2025-CIS-GPO` | GPO backup. Unchanged. |

Leaving any of these set on this role is harmless - they are simply unused - but
nothing will create a GPO until you run the GPO role.

If you harden a domain, run both: this role against each member server and domain
controller for the host scoped controls, and the GPO role once against a domain
controller for the Account Policy controls this one reports as skipped.

## Auditing (beta)

**The audit component is in beta while we gather feedback.** It is usable and its
results are meaningful, and we would like to hear how it behaves on your estate.
Please raise an issue, or come and talk to us on the [Discord Server](https://www.lockdownenterprise.com/discord).
Remediation is unaffected - `run_audit` and `setup_audit` both default to `false`,
so none of this runs unless you ask for it.

This role is paired with [Windows-2025-CIS-Audit], a set of syver specs generated
from this role by `scripts/generate_windows_audit.py`, so the audit asserts what
the role actually does rather than a separately maintained restatement of it.

### Switches

| Variable | Default | Effect |
|---|---|---|
| `setup_audit` | `false` | Place the syver binary and the audit content on the host |
| `run_audit` | `false` | Run the audit before and after remediation |
| `audit_only` | `false` | Run the pre-remediation audit, then stop without remediating |
| `fetch_audit_output` | `false` | Collect the result files after the run |

```bash
# stage the binary and content, remediate nothing
ansible-playbook site.yml -e setup_audit=true

# audit the host and stop
ansible-playbook site.yml -e run_audit=true -e audit_only=true

# remediate with a before and after audit
ansible-playbook site.yml -e run_audit=true
```

### Requirements

`syver.exe` is never committed to this repository. The default
`get_audit_binary_method: download` fetches it from the public release named in
`audit_bin_version` (v0.11.1) and verifies the published SHA256. Keep that digest
set: `win_get_url` checks it, and that check is what stands between a compromised
mirror and every audited host running someone else's binary elevated. No Windows
ARM64 asset is published for v0.11.1, so `ARM64_checksum` is deliberately empty
and a download on such a host fails the lookup rather than fetching an amd64
binary.

To supply your own build instead, set `get_audit_binary_method: copy` and point
`audit_bin_copy_location` at it; it defaults to `syver-windows-amd64.exe` in this
role directory.

The audit content comes from `audit_conf_source`, which defaults to the sibling
`Windows-2025-CIS-Audit` checkout on the controller.

## Documentation

- [Read The Docs](https://ansible-lockdown.readthedocs.io/en/latest/)
- [Getting Started](https://www.lockdownenterprise.com/docs/getting-started-with-lockdown#GH_AL_WINDOWS_2025_cis)
- [Customizing Roles](https://www.lockdownenterprise.com/docs/customizing-lockdown-enterprise#GH_AL_WINDOWS_2025_cis)
- [Per-Host Configuration](https://www.lockdownenterprise.com/docs/per-host-lockdown-enterprise-configuration#GH_AL_WINDOWS_2025_cis)
- [Getting the Most Out of the Role](https://www.lockdownenterprise.com/docs/get-the-most-out-of-lockdown-enterprise#GH_AL_WINDOWS_2025_cis)

## Requirements

**General:**

- Basic knowledge of Ansible, below are some links to the Ansible documentation to help get started if you are unfamiliar with Ansible

  - [Main Ansible documentation page](https://docs.ansible.com)
  - [Ansible Getting Started](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)
  - [Tower User Guide](https://docs.ansible.com/ansible-tower/latest/html/userguide/index.html)
  - [Ansible Community Info](https://docs.ansible.com/ansible/latest/community/index.html)
- Functioning Ansible and/or Tower Installed, configured, and running. This includes all of the base Ansible/Tower configurations, needed packages installed, and infrastructure setup.
- Please read through the tasks in this role to gain an understanding of what each control is doing. Some of the tasks are disruptive and can have unintended consequences in a live production system. Also, familiarize yourself with the variables in the defaults/main.yml file.

**Technical Dependencies:**

- Windows 2025 - Other versions are not supported
- Python3 Ansible run environment
- passlib
- xmltodict
- pywinrm or pypsrp

## Role Variables

This role is designed so that the end user should not have to edit the tasks themselves. All customizing should be done via the defaults/main.yml file or with extra vars within the project, job, workflow, etc.

## Tags

There are many tags available for added control precision. Each control has its own set of tags noting what level, what OS element it relates to, whether it's a patch or audit, and the rule number. Additionally, NIST references follow a specific conversion format for consistency and clarity.

### Conversion Format for NIST References:

  1. Standard Prefix:

    - All references are prefixed with "NIST".

  2. Standard Types:

    - "800-53r5" references are formatted as NIST800-53R5 (with 'R' capitalized). Only Rev 5 is tagged.
    - "800-171" references are formatted as NIST800-171.

  3. Details:

    - Section and subsection numbers use periods (.) for numeric separators.
    - Parenthetical elements are separated by underscores (_), e.g., IA-5(1)(d) becomes IA-5_1_d.
    - Subsection letters (e.g., "b") are appended with an underscore.

### Example of Tag Usage:
Below is an example of the tag section from a control within this role. Using this example, if you set your run to skip all controls with the tag smb, this task will be skipped. Conversely, you can choose to run only controls tagged with smb.

```sh
      tags:
        - level1-domaincontroller
        - level1-memberserver
        - rule_18.3.3
        - patch
        - smb
        - NIST800-171_3.4.2
        - NIST800-171_3.5.2
        - NIST800-53R5_CM-6_b
        - NIST800-53R5_AC-2
        - NIST800-53R5_AC-17_2
        - NIST800-53R5_IA-5_1_d
```

### Conversion Examples in Use:
  - 800-53r5 IA-5(1)(d) NIST800-53R5_IA-5_1_d
  - 800-53r5 AC-17(2) NIST800-53R5_AC-17_2
  - 800-53r5 CM-6b. NIST800-53R5_CM-6_b
  - 800-171 3.5.2 NIST800-171_3.5.2

By maintaining this consistent tagging structure, it becomes easier to filter and manage tasks based on specific controls and compliance requirements.

## Community Contribution

We encourage you (the community) to contribute to this role. Please read the rules below.

- Your work is done in your own individual branch. Make sure to Signed-off-by and GPG sign all commits you intend to merge.
- All community Pull Requests are pulled into the devel branch
- Pull Requests into devel will confirm your commits have a GPG signature, Signed-off-by, and a functional test before being approved
- Once your changes are merged and a more detailed review is complete, an authorized member will merge your changes into the main branch for a new release

## Pipeline Testing

uses:

- ansible-core 2.16
- ansible collections - pulls in the latest version based on requirements file
- runs the audit using the devel branch
- This is an automated test that occurs on pull requests into devel
- self-hosted runners using OpenTofu

## Local Testing

  - Ansible
    - ansible-core 2.21.01- python 3.13

## Credits and Thanks

Massive thanks to the fantastic community and all its members.

This includes a huge thanks and credit to the original authors and maintainers.

[Windows-2025-CIS-Audit]: https://github.com/ansible-lockdown/Windows-2025-CIS-Audit
