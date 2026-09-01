# ChangeLog

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

## Release 1.3.0

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

## Release 1.2.0

July 2026
  - 2.2.22 has been updated to reflect the correct values in remediation and the GPO creation (Guests and S-1-5-114) Thanks -Evil-

## Release 1.1.0

April 2026
  - Updated the cloud based system check for manual overrides. New variable now in the defualt main. Please read the comments for the new variable. 
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

## Release 1.0.0

September 2025
  - Updated Control 18.4.6 When Statement In Ansible And GPO
  - Updated 2.3.10.10 Ansible Title
  - Updated the Ansible Task For 2.3.6.5
  - Updated 18.10.43.11.1.1.2 Title
  - Updated 18.10.56.1 Title to remove L2
  - Removed Block from Ansible 18.10.43.13.1

June 2025
  - This Release is based on CIS Benchmark v1.0.0
