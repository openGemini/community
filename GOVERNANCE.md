# openGemini Governance

The openGemini project is dedicated to creating a high-performance, low-cost enterprise-level time-series database for all users.
This governance explains how the project is run.

- [Values](#values)
- [Maintainers](#maintainers)
- [Becoming a Maintainer](#becoming-a-maintainer)
- [Committers](#committers)
- [Becoming a Committer](#becoming-a-committer)
- [Meetings](#meetings)
- [openGemini & CNCF](#opengemini-and-cncf)
- [Code of Conduct](#code-of-conduct)
- [Security Response Team](#security-response-team)
- [Voting](#voting)
- [Other Projects](#other-projects)
- [Modifications](#modifying-this-charter)

## Values

The openGemini and its leadership embrace the following values:

- Openness: Communication and decision-making happens in the open and is discoverable for future reference. As much as possible, all discussions and work take place in public forums and open repositories.
- Fairness: All stakeholders have the opportunity to provide feedback and submit contributions, which will be considered on their merits.
- Community over Product or Company: Sustaining and growing our community takes priority over shipping code or sponsors' organizational goals. Each contributor participates in the project as an individual.
- Inclusivity: We innovate through different perspectives and skill sets, which can only be accomplished in a welcoming and respectful environment.
- Participation: Responsibilities within the project are earned through participation, and there is a clear path up the contributor ladder into leadership positions.

## Maintainers

openGemini Maintainers have write access to the project GitHub repository such as [openGemini](https://github.com/openGemini/openGemini), [openGemini-operator](https://github.com/openGemini/openGemini-operator),  [opengemini-client-go](https://github.com/openGemini/opengemini-client-go), [opengemini-client-java](https://github.com/openGemini/opengemini-client-java) etc. They can merge their own patches or patches from others. The current maintainers can be found in [MAINTAINERS.md](./MAINTAINERS.md). Maintainers collectively manage the project's resources and contributors.

This privilege is granted with some expectation of responsibility: maintainers are people who care about the openGemini project and want to help it grow and improve. A maintainer is not just someone who can make changes, but someone who has demonstrated their ability to collaborate with the team, get the most knowledgeable people to review code and docs, contribute high-quality code, and follow through to fix issues (in code or tests).

A maintainer is a contributor to the project's success and a citizen helping the project succeed.

Currently, the collective team of all Maintainers is known as the Maintainer Council, which is the governing body for the project.

### Becoming a Maintainer

To become a Maintainer you need to demonstrate the following:

- commitment to the project:
  - participate in discussions, contributions, code and documentation reviews for openGemini or more,
  - perform reviews for openGemini non-trivial pull requests,
  - contribute openGemini non-trivial pull requests and have them merged,
  - independently develop at least one feature
- ability to write quality code and/or documentation,
- ability to collaborate with the team,
- understanding of how the team works (policies, processes for testing and code review, etc),
- understanding of the project's code base and coding and documentation style.
- having served as a Committer for at least six months

A new Maintainer must be proposed by at least two existing maintainer by sending a message to the [developer mailing list](https://groups.google.com/g/openGemini). A simple majority vote of existing Maintainers approves the application. Maintainers who are selected will be granted the necessary GitHub rights. 

## Committers

openGemini Committers have write access to the corresponding project GitHub repository. They can merge the patches from others. A Committer need to review code to maintain/improve code quality, confirming and handling PR Review requests from community members, approving relevant code contributions to be merged into the main branch and making long-term contributions while guiding other community Contributors/members to contribute to the project.

### Becoming a Committer

To become a Committer you need to demonstrate the following:

- recommended by at least 2 maintainers,
- review of PRs  or code review for at least 3 months,
- continuous contribution to the community for at least 6 months,
- ability to write quality code and/or documentation,
- ability to collaborate with the team,
- understanding of how the team works (policies, processes for testing and code review, etc),
- understanding of the project's code base and coding and documentation style.

## Removing a Maintainer or Committer

Maintainers/Committers may resign at any time if they feel that they will not be able to continue fulfilling their project duties.

Maintainers/Committers may also be removed after being inactive, failure to fulfill their Maintainer responsibilities, violating the Code of Conduct, or other reasons. Inactivity is defined as a period of very low or no activity in the project for a year or more, with no definite schedule to return to full Maintainer activity.

A Maintainer/Committer may be removed at any time by a 2/3 vote of the remaining maintainers.

## Meetings

Regular Community Meeting:

- Asia Time: **Friday at 15:00-16:00 (UTC+8)** (biweekly).
  ([Convert to your timezone.](https://www.thetimezoneconverter.com/?t=16%3A30&tz=GMT%2B8&))

Resources:

- [Meeting notes and agenda](https://docs.qq.com/doc/DU1l0T01aTXlCQXJG)
- [Meeting link]()
- [Meeting Calendar]() | [Subscribe]()

Maintainers will also have closed meetings in order to discuss security reports or Code of Conduct violations. Such meetings should be scheduled by any Maintainer on receipt of a security issue or CoC report. All current Maintainers must be invited to such closed meetings, except for any Maintainer who is accused of a CoC violation.

## openGemini and CNCF

openGemini is donating to CNCF

## Code of Conduct

The [openGemini Code of Conduct](https://github.com/openGemini/community/blob/main/code_of_conduct.md) is aligned with the CNCF Code of Conduct.

## Security Response Team

The Maintainers will appoint a Security Response Team to handle security reports. This committee may simply consist of the Maintainer Council themselves. If this responsibility is delegated, the Maintainers will appoint a team of at least two contributors to handle it. The Maintainers will review who is assigned to this at least once a year.

The Security Response Team is responsible for handling all reports of security holes and breaches according to the [security policy](./security-policy.md).

## Voting

While most business in openGemini is conducted by "[lazy consensus](https://community.apache.org/committers/lazyConsensus.html)", periodically the Maintainers may need to vote on specific actions or changes. A vote can be taken on the developer mailing list for security or conduct matters.Votes may also be taken at the developer meeting. Any Maintainer may demand a vote be taken.

Most votes require a simple majority of all Maintainers to succeed, except where otherwise noted. Two-thirds majority votes mean at least two-thirds of all existing maintainers.

## Other Projects

The openGemini organization is open to receive new sub-projects under its umbrella. To accept a project
into the __openGemini__ organization, it has to meet the following criteria:

- Must be licensed under the terms of the Apache License v2.0
- Must be related to one or more scopes of the openGemini ecosystem:
  - openGemini project artifacts (website, deployments, CI, etc)
  - Solutions that include openGemini
  - Other AI/BigData related processing
- Must be supported by a Maintainer not associated or affiliated with the author(s) of the sub-projects

The submission process starts as a Pull Request or Issue on the
[openGemini/openGemini](https://github.com/openGemini/openGemini) repository with the required information
mentioned above. 

## Modifying this Charter

Changes to this Governance and its supporting documents may be approved by a 2/3 vote of the Maintainers.