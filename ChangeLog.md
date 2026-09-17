# ChangeLog

## CIS Benchmark v1.0.0
## September 2026 Updates - audit content

  - fixed to new vars for section 5
  - removed incorrect setting in compliance template
  - tidy up audit pulling in more than once
  - 18.9.25.5 LAPS PasswordLength is written again
    - Its when: compared the variable with itself, so the write never ran
  - docs: README table of the controls the domain overrides, and how the role and audit report them
  - audit_content copy and archive remove the previous audit content first
    - Old spec folders left on the host after a rename loaded twice and overwrote each other
  - Audit summary capture fails when the results file does not parse or the summary is empty
    - Was an empty summary with the play still passing
  - Moved the contents of tasks/ansible_hardening up into tasks/
  - Merged tasks/ansible_hardening/main.yml into tasks/main.yml
    - Section and post imports unchanged
  - Updated the prelim include path and the defaults comment that named it
  - Grouped section tasks as tasks/section_N/, each with a main.yml
  - 2.3.11.6 skipped on domain joined hosts, with a warning
    - ForceLogoffWhenHourExpire is a [System Access] value the Default Domain Policy sets to 0
  - README domain members section covers 2.3.11.6
  - 2.3.7.4 legal notice written through the security database
    - Was cut to its first line whenever a later win_security_policy task re-imported
    - Blank lines dropped, commas kept
  - 1.2.1 lockout duration applies on a stock host
    - The reset counter reconcile moved from the prelim into section 1, after 1.2.2 enables lockout
    - The prelim read both values while lockout was still disabled, so the decision was stale
    - Windows auto-populates duration and reset to 30 once 1.2.2 runs, which rejected the 1.2.1 import
  - A tagged run no longer touches account policy
    - The reconcile used to sit in the prelim under tags: always, so any --tags run lowered the reset counter
    - It now carries the rule_1.2.1 tags, so only a run that asks for 1.2.1 can move it
  - secedit reads work under Constrained Language Mode
    - Replaced the System.IO.Path and array .NET calls in 1.2.1, 2.2.31 and the audit summary readers
    - Cmdlets, operators and $Matches only
    - secedit now exports to a path that does not already exist, which keeps the [Unicode] header
  - Standalone detection covers a stand-alone workstation, not only a stand-alone server
  - Domain scope warning names the Default Domain Policy and the Windows-2025-CIS-GPO role
    - States that a domain controller has no local remediation path for section 1
  - README: what the role enforces on a standalone, member server and domain controller
  - README: why a control's CIS profile tags and its when can disagree, using 1.2.3
  - README: migration path for the GPO variables removed from this role
  - moved reading user hive fromPerlim to section19/main.yml to minimise potential unload hive issue
  - NIST800-53 task tags removed, NIST800-53R5 and NIST800-171 kept
  - README tagging example shows NIST800-53R5 only

## September 2026 Updates - GPO creation removed

  - Removed the GPO creation path (tasks/gpo_creation)
  - Removed the test domain creation (tasks/domain_creation)
  - Removed the GPO templates (templates/cis_templates, templates/windows_templates)
  - Removed the GPO pipeline workflows and badges
  - Removed win25cis_ansible_remediation and win25cis_create_gpos toggles
  - Removed the GPO, domain creation, DSRM and GPO backup defaults
  - Removed the GPO name vars
  - Removed the gpo galaxy tag
  - Removed the GPO sections from the README
  - Removed GPO entries from .secrets.baseline
  - GPO creation now lives in Windows-2025-CIS-GPO
QA pass against CIS Microsoft Windows Server 2025 Benchmark v1.0.0.

  - docs: README explains why section 1 cannot hold on a domain member, with the measured before and after values
  - Corrected GPO tasks that were gated by a neighbouring control's toggle.
  - Corrected 18.9.39.1 in the GPO creation path.
  - Corrected two when conditions that could never be true, because a when list is evaluated as AND.
    - 18.9.25.8 GPO was gated on the value being both 3 and 5, so the registry write never ran.
    - The 18.9.25.7 GPO warning tasks were gated on the value being both greater than 8 and 0.
  - Removed the 1.1.2 GPO sub-task that wrote Netlogon\Parameters MaximumPasswordAge.
    - That value is control 2.3.6.5 and it was being written with the user password age variable
      rather than the machine account password age variable.
  - Corrected task titles that named a different control, verified against the benchmark.
  - Corrected rule_ tags
  - Corrected level tags
  - Added the missing patch tags to 17.3.1 GPO & 18.6.4.3 GPO.
  - Removed 11 stray gpo tags from the hardening path, where --tags gpo selected direct registry
    writes in sections 1 and 18.
  - Normalised 128 malformed NIST tags.
  - Corrected the credentialsdelecation tag to credentialsdelegation.
  - Corrected the 1.1.2 and 1.1.3 conditionals
  - Removed the cloud-based task ordering split for the account lockout controls.
    - Deleted tasks/ansible_hardening/section01_cloud_lockout_order.yml, which duplicated
      1.2.1 to 1.2.4 purely to run them in a different order.
    - Removed hosted_virtual_system_override and prelim_win25cis_cloud_based_system. The
      auto-detection used ansible_virtualization_type and ansible_system_vendor and its own
      documentation listed VMware vSphere, AWS GovCloud and standalone VMware as known failures.
    - Root cause: Windows requires LockoutDuration to be no less than ResetLockoutCount, and
      win_security_policy re-imports the whole secedit ini on every call, so whichever of 1.2.1 or
      1.2.4 is written first fails with "The parameter is incorrect" when the existing values
      conflict. This is ansible/ansible#62594. It depends on the starting state of the image, not
      on whether the host is cloud based.
    - The prelim now reads the current LockoutDuration and ResetLockoutCount and lowers the reset
      counter only when it would block 1.2.1. Windows maintains duration >= reset, so that makes
      the fixed order 1.2.2, 1.2.1, 1.2.4, 1.2.3 valid from any starting state.
  - Migrated the remaining 12 bare fact references to ansible_facts bracket notation.
    - ansible_distribution, ansible_distribution_major_version and ansible_os_family in tasks/main.yml.
    - ansible_virtualization_type and ansible_system_vendor in the prelim cloud detection.
    - ansible_date_time in the GPO backup zip name.
  - Corrected block headers labelled AUDIT that perform remediation
  - Added no_log to the domain promotion task, which interpolated the DSRM password into a shell body.
  - Reordered 59 tasks into the canonical key order.
  - Aligned min_ansible_version in defaults/main.yml with meta/main.yml (2.16.1).
  - Removed the duplicate windows2025 galaxy tag from meta/main.yml.
  - Removed export_badges_public.yml and update_galaxy.yml, update_galaxy.yml carried no repository_visibility guard.
  - Updated actions/checkout to v7.0.0 across the workflows.
  - Updated welcome message and version for workflows
  - Updated the CONTRIBUTING header to Contributing to Ansible-Lockdown Projects.
  - Updated the README social badge from Twitter to X.
  - Updated .gitignore
  - Moved min_ansible_ver to vars/main from defaults
  - Split the five task files over 100 tasks into per CIS subsection files.
    - section02, section17, section18 in the hardening path; gpo_section02, gpo_section18 in the GPO path.
    - sectionNN_main.yml dispatcher imports the per subsection files; content byte identical.
  - Prelim account lockout read now defaults missing secedit values to 0.
    - Was empty on an unconfigured host, failing the set_fact and aborting the run.
  - 2.2.31 Generate security audits made idempotent.
    - Reads current holders and only reapplies win_user_right when they differ.
  - 2.3.11.6 Force logoff when logon hours expire now uses the mechanism that implements it.
    - Both paths wrote LanManServer\Parameters EnableForcedLogOff, which is control 2.3.9.4's
      value. CIS names no registry location for 2.3.11.6 because it is a secedit [System Access]
      setting.
    - Host proven: ForceLogoffWhenHourExpire was 0 on both test hosts while the control reported
      applied, and is 1 after the fix.
    - Hardening path now uses win_security_policy; GPO path writes GptTmpl.inf [System Access],
      matching sections 1 and 2.2.
  - Added check_mode: false to the seven read-only tasks that register a variable.
    - prelim.yml lockout policy and interactive user hives, 2.2.31, the two 2.3.10.9 feature
      checks, and both section 5 spooler checks.
    - Without it a --check run skipped the read, and the next task dereferenced .stdout on a
      skipped result. The play aborted at task 10 of 1130 on every host.
    - 34 other read-only registers already had it; these seven were the gap.
  - Corrected the 18.10.13.2 level tags to level2 in both paths, matching the benchmark profile.
  - Fixed the 2.2.31 idempotency guard, which read nothing and so never suppressed the write.
    - secedit exports USER_RIGHTS without the [Unicode] header when the target file already
      exists, and GetTempFileName creates it. Get-Content then reads the UTF-16 export as
      single-byte characters, nothing matches, and secedit still exits 0.
    - The file is now removed before the export. Host verified: the read returns
      S-1-5-19,S-1-5-20 and the control no longer reapplies on every run.
    - The prelim SECURITYPOLICY read uses the same pattern and is not affected; only the
      USER_RIGHTS export is, and there is one in the role.
  - Removed the domain member gate from 2.2.11, which contradicted the benchmark and the role itself.
    - Host verified on a real PDC: SeBackupPrivilege went from Administrators, Server Operators,
      Backup Operators to Administrators only.
    - v1.0.0_recommedations.json lists Back up files and directories as Level 1 Domain Controller
      and Level 1 Member Server, the task's own tags say the same, and the GPO path writes it to
      both the L1 DC and L1 MS GPOs. Only the hardening path gated it on
      prelim_win25cis_is_domain_member, so it never applied on a domain controller.
    - Checked the other 34 controls carrying that gate against the JSON: all 34 are Member Server
      only in the benchmark, so their gates are correct and were left alone.
  - Corrected --tags create_domain, which included the file and then filtered out every task in it.
    - Tags do not propagate through a dynamic include; added apply: to that include and to the
      gpo, domain include beside it.
  - Removed the duplicated AD-Domain-Services install task in the domain creation prelim.
  - Replaced the flat 360 second post-promotion pause with wait_for_connection.
    - pause needs a controlling terminal and stalls indefinitely without one, so an automated run
      hung there. The verification task now retries until ADWS answers instead.
  - Renamed the 2.2.31 register from win25cis_2_2_31_current to rule_2_2_31_current.
    - A register is not a role variable and must not carry the role variable prefix.
  - Removed unicode punctuation from warning_facts.yml and the prelim comment box.
  - Changed exchange, iis and hypver V dynamic discovery

## CIS Benchmark v1.0.0
## August 2026 Updates

  - Updated LICENSE year to 2026 and company to MindPoint Group - A Quantum Sky Company.
    - The public repo still carries the former company name and is corrected on promotion.
  - Updated run banner and meta/main.yml with the current company name.
  - Updated meta/main.yml author to Ansible-Lockdown Team and min_ansible_version to 2.16.1.
  - Updated 2.3.10.9 to use ansible.windows.win_feature_info. Replacing the deprecated community.windows redirect
  - Updated task names in sections 2, 17 and 18 to match the CIS control wording.
  - Corrected repeated words in README.md and two variable comments in defaults/main.yml.
    - Grammar and spelling findings were identified with the Repo QA Checker.
  - Added the missing gpo tag to 18.6.7.4 in the GPO creation tasks.
  - Restored the #81 credit under Release 1.1.0.
  - Aligned the prelim cloud-detection task name casing with the public repo.
  - Corrected section 18.6.8 against CIS v1.0.0.
    - Removed the duplicate 18.6.8.4 Enable authentication rate limiter, which repeated 18.6.7.4.
    - Renumbered 18.6.8.5 to 18.6.8.4, 18.6.8.6 to 18.6.8.5 and 18.6.8.7 to 18.6.8.6.
    - Restored Require Encryption as 18.6.8.7. Release 1.3.0 removed it as 18_6_8_8 rather than
      renumbering it, which dropped the control from the remediation and GPO paths.

## Based on CIS v1.0.0
July 2026
  - Removed win25cis_rule_18_10_16_8.
  - Removed win25cis_rule_18_6_8_8.
  - Added win25cis_rule_18_6_7_4 causing existing controls to be renumbered to 18_6_7_7.
  - Updated win25cis_rule_18_9_25_2 registry value name.
  - Issues Addressed:
    - [#26](https://github.com/ansible-lockdown/Windows-2025-CIS/issues/26) - Thanks @dwierima-at
    - [#25](https://github.com/ansible-lockdown/Windows-2025-CIS/issues/25) - Thanks @dwierima-at
    - [#24](https://github.com/ansible-lockdown/Windows-2025-CIS/issues/24) - Thanks @dwierima-at
    - [#23](https://github.com/ansible-lockdown/Windows-2025-CIS/issues/23) - Thanks @dwierima-at

## CIS Benchmark v1.0.0
July 2026
  - 2.2.22 has been updated to reflect the correct values in remediation and the GPO creation (Guests and S-1-5-114) Thanks -Evil-

## CIS Benchmark v1.0.0
April 2026
  - Updated the cloud based system check for manual overrides. New variable now in the default main. Please read the comments for the new variable.
  - Updated 18.10.57.3.10.1 variable accept anything between 1 and 900000 in Hardening & GPO.
  - Updated Section 2 GPO for win_skip_for_test controls. Read comments in default/main.
  - Issues Addressed:
    - [#2](https://github.com/ansible-lockdown/Windows-2025-CIS/issues/2) - Thanks @davidstanaway
    - [#7](https://github.com/ansible-lockdown/Windows-2025-CIS/issues/7) - Thanks @R2J2 (Updated When Statement to take into account Bool now)
    - [#86](https://github.com/ansible-lockdown/Windows-2022-CIS/issues/86) - Thanks @git-cgallagher (Windows 2022 Issue Added Here To Update 2025)
    - [#84](https://github.com/ansible-lockdown/Windows-2022-CIS/issues/84) - Thanks @Randriy-bulynko (Windows 2022 Issue Added Here To Update 2025)
    - [#87](https://github.com/ansible-lockdown/Windows-2022-CIS/issues/87) - Thanks @Randriy-bulynko (Windows 2022 Issue Added Here To Update 2025)
    - [#83](https://github.com/ansible-lockdown/Windows-2022-CIS/issues/83) - Thanks @exu-g (Windows 2022 Issue Added Here To Update 2025)
    - [#81](https://github.com/ansible-lockdown/Windows-2022-CIS/issues/81) - Thanks @davidstanaway (Windows 2022 Issue Added Here To Update 2025)
  - PR's Addressed:
    - [#3](https://github.com/ansible-lockdown/Windows-2025-CIS/pull/3) - Thanks @MatthieuLeboeuf

## CIS Benchmark v1.0.0
September 2025
  - Updated Control 18.4.6 When Statement In Ansible And GPO
  - Updated 2.3.10.10 Ansible Title
  - Updated the Ansible Task For 2.3.6.5
  - Updated 18.10.43.11.1.1.2 Title
  - Updated 18.10.56.1 Title to remove L2
  - Removed Block from Ansible 18.10.43.13.1

June 2025
  - This Release is based on CIS Benchmark v1.0.0
