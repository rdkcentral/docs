# New Repository and Repository Access Request FAQ
- [GitHub New Repository Request](#gitHub-new-repository-request)
- [Gerrit New Repository Request](#gerrit-new-repository-request)
- [GitHub Private Repository Access Request](#github-private-repository-access-request)
- [GitHub Elevated Privileges Request](#github-elevated-privileges-request)
- [Gerrit Repository Access Request](#gerrit-repository-access-request)

## GitHub New Repository Request
If you require a new repository on **RDK Central GitHub**, you must create a [GHREPOREQ](https://jira.rdkcentral.com/jira/issues/?jql=project%20%3D%20GHREPOREQ) ticket (one ticket per repository).

If the request is for a GitHub repository, the owner and maintainers must also ensure:

1. [GitHub Repo Access Setup Complete](https://wiki.rdkcentral.com/spaces/CMF/pages/347342833/RDK+Central+Github+Profile+Setup+for+Private+Repository+Access) (Yes/No)
2. Users Accepted GitHub Invites (Yes/No)
3. Ensure you specify the **public GitHub IDs** and/or **GitHub Teams** of those who require access and what **level of access** they need.
Reference: https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization

New repos created on **RDKCentral GitHub** https://github.com/rdkcentral/ should generally be created privately initially and only open sourced when license, code and copyrights have been approved by the RDKM compliance team.

- All contributions to Open Sourced repos have to be made using a Pull Request which in turn triggers various checks for licensing and copyright issues.
- Private repos will allow for direct push by the component owner.
- When a private repo is to be open sourced the repo will first be scanned to ensure no license or copyright issues exist. Any that are found must be resolved before the repo is opensourced.
So if a component owner wishes to populate a new repo using direct push then the new repo request should stipulate the repo be created privately with initial access restricted to the component owners.
- Once the initial population of the repo is complete a new ticket should be opened requesting the repo be open sourced - this will then be scanned allowing licensing issues to be resolved prior to open sourcing.


## Gerrit New Repository Request
To create a new repository on **RDK Central Gerrit**, raise a **CMFSUPPORT** ticket and select **New Repository Request** ticket type.

![New Rep Jira](https://github.com/rdkcentral/docs/tree/main/docs/support/images/NewRepo-Jira.png)

**Each repo requires its own ticket.**

- **Component Name** (Name of the repository):
- **Component Description** (To appear with the repository):
- **Owners/Maintainers List** (include LDAP groups owners/approvers):
- **Master Location & Sync** (if applicable): RDKCentral Gerrit [code.rdkcentral.com](https://code.rdkcentral.com)
- **Master Git URL** (if applicable): if importing from another source
- **History Sharing Allowed:** (Can history be shared): Yes/No/NA
- **License** (Apache 2.0, RDK License, etc.): What is the license to be used in the repo
- **Required Branches** (e.g., develop/main): If using GitFlow you wil need develop/main, do you need any other branches?
- **Initial Access Restriction:** (if any)(No / Restricted to Comcast, another partner name): NA/No/Yes (if yes who should it be restricted to)
- **Export Compliance:** (Does the code contain any SSL, Encryption, or Security related implementation?): Yes/No/NA
- **CI Support:** (Yes/No/NA): for example will you be adding L1/L2 tests, coverity, will the repo be part of a RDK build?
- **Integrate on Reference Platform:** (Yes/No): Only applicable for repos that are part of the RDK-E or RDK-B builds
- **Profile()s:** (RDK-V, RDK-E, RDK-B, RDK-C):

If requesting a Gerrit repo (https://code.rdkcentral.com), then the owners and approvers must be specified.
- Identify the LDAP groups which should have access to the repository or if required:
   - Request LDAP group creation for owners and approvers via support@rdkcentral.com. e.g. [ldap/collab_automatics_owner](https://code.rdkcentral.com/r/admin/groups/ldap:cn%3Dcollab_automatics_owner%2Cou%3DGroups%2Cdc%3Drdkcentral%2Cdc%3Dcom) & [ldap/collab_automatics_approver](https://code.rdkcentral.com/r/admin/groups/ldap:cn%3Dcollab_automatics_approver%2Cou%3DGroups%2Cdc%3Drdkcentral%2Cdc%3Dcom)

## GitHub Private Repository Access Request
To request access to a private (restricted) RDK Central GitHub repository:
1. Complete [RDK Central GitHub Profile Setup for Private Repository Access](https://wiki.rdkcentral.com/spaces/CMF/pages/347342833/RDK+Central+Github+Profile+Setup+for+Private+Repository+Access)
2. Raise a **CMF-SUPPORT** ticket specifying:
   - **Repositories** requiring access
   - **Approval** from the repository owner
   - Your **public GitHub ID**
3. Specify the **public GitHub IDs** who require access and required **access levels**, refer to https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization
4. Refer to the [GitHub Access Issues FAQ](https://wiki.rdkcentral.com/spaces/CMF/pages/355767084/RDK+Central+Github+Access+Issues+FAQ) if if any questions about the repository access process.

## GitHub Elevated Privileges Request
For elevated privileges on a private repository:
1. Complete [GitHub Profile Setup for Elevated Privileges on a Repository](https://wiki.rdkcentral.com/spaces/CMF/pages/348390621/RDK+Central+Github+Profile+Setup+for+Elevated+Privileges+on+a+Repository).
2. Raise a **CMF-SUPPORT** ticket specifying:
   - **Which Repositories** you require priveleges
   - **Privileges requested**
   - Owner **approval** from the repository owner
   - Your **public GitHub ID**
3. Specify the **public GitHub IDs** and required elevated **access level**, refer to https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization
4. Refer to the [GitHub Access Issues FAQ](https://wiki.rdkcentral.com/spaces/CMF/pages/355767084/RDK+Central+Github+Access+Issues+FAQ) if any questions about the repository access process.

## Gerrit Repository Access Request
Gerrit access is controlled via LDAP.

Steps:
1. Contact the repository owner for approval and LDAP group details.
2. Once you have approval, Email support@rdkcentral.com to be added to the LDAP group.
3. If unsure who the owner is, raise a **CMF-SUPPORT** ticket and we can check.
