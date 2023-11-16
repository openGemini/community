# Security Policy

## Report a vulnerability

We sincerely request you to keep the vulnerability information confidential and responsibly disclose the vulnerabilities.

To report a vulnerability, please contact the Security Team: [opengemini.ts@opengemini.org](mailto:opengemini.ts@opengemini.org). You can email the Security Team with the security details and the details expected for [openGemini bug reports](https://github.com/openGemini/openGemini/issues/new/choose).

The information of the Security Team members is described as follows:

| Name                                                      | Email                                                 |
| --------------------------------------------------------- | ----------------------------------------------------- |
| Yu Xiang ([@xiangyu5632](https://github.com/xiangyu5632)) | [xiangyu9@huawei.com](mailto:xiangyu9@huawei.com)     |
| Shilin Li ([@shilinlee](https://github.com/shilinlee))    | [shilin.lee@huawei.com](mailto:shilin.lee@huawei.com) |

### E-mail Response

The team will help diagnose the severity of the issue and determine how to address the issue. The reporter(s) can expect a response within 2 business day acknowledging the issue was received. If a response is not received within 2 business day, please reach out to any Security Team member directly to confirm receipt of the issue. We’ll try to keep you informed about our progress throughout the process.

### When Should I Report a Vulnerability?

- You think you discovered a potential security vulnerability in openGemini
- You are unsure how a vulnerability affects openGemini

### When Should I NOT Report a Vulnerability?

- You need help tuning openGemini components for security
- You need help applying security related updates
- Your issue is not security related

If you think you discovered a vulnerability in another project that openGemini depends on, and that project has their own vulnerability reporting and disclosure process, please report it directly there.

## Security release process

The openGemini community will strictly handle the reporting vulnerability according to this [procedure](./security-release-process.md). The following flowchart shows the vulnerability handling process.

[![img](https://github.com/kubeedge/community/raw/master/team-security/images/Vulnerability-handling-process.PNG)](https://github.com/kubeedge/community/blob/master/team-security/images/Vulnerability-handling-process.PNG)

## Relative Mailing lists

- [opengemini@googlegroups.com](mailto:opengemini@googlegroups.com), is for reporting security concerns to the openGemini Security Team, who uses the list to privately discuss security issues and fixes prior to disclosure.

## Supported versions

openGemini versions are expressed as x.y.z, where x is the major version, y is the minor version, and z is the patch version, following [Semantic Versioning](https://semver.org/) terminology.

The openGemini project maintains release branches for the most recent three minor releases. Applicable fixes, including security fixes, may be backported to those three release branches, depending on severity and feasibility.

Our typical patch release cadence is every 3 months. Critical bug fixes may cause a more immediate release outside of the normal cadence. We also aim to not make releases during major holiday periods.

See the [openGemini releases page](https://github.com/openGemini/openGemini/releases) for information on supported versions of openGemini.
