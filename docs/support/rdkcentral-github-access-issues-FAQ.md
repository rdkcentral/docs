## RDK Central Github Access Issues FAQ


[General Information](#general-information)

[Clone Issues](#clone-issues)

[Private Repository Access and GitHub Invite Issues](#private-repository-access-and-github-invite-issues)

[Working with multiple GitHub Organisations and Accounts](#working-with-multiple-github-organisations-and-accounts)

[PR flagged as not having accepted CLA agreement](#pr-flagged-as-not-having-accepted-cla-agreement)

___

# General Information
Please Note RDK Central Github https://github.com/rdkcentral uses a pubic Github ID handle

If you have multiple GitHub accounts, e.g. public account(s) and a corporate enterprise account use the GitHub account switcher to make sure you are on the correct account before making any changes, just click the account switcher arrows.

![Switcher](https://github.com/rdkcentral/docs/tree/main/docs/support/images/GithubAccountSwitcher.png)


If you raise a support ticket please ensure you tell us your **public GitHub username** and any **repositories** you are having issues accessing.

___

# Clone Issues
To access rdkcentral GitHub repositories without having to enter your username/password you must create a PAT and set this in your ~/.netrc


```
machine github.com
   login <Public GitHub userID>
    password <PAT>

```

To create a PAT navigate to Settings - Developer Settings - Personal access tokens - Tokens (classic)

https://github.com/settings/profile ← click on "Developer settings" here

Note: for git actions (clone/push/pr) you need to select at least the following scopes:

repo write:packages

![Scope Selection](https://github.com/rdkcentral/docs/tree/main/docs/support/images/PAT-settings.png)

___

# Private Repository Access and GitHub Invite Issues
The following things should be checked if you are having issues accessing rdkcentral GitHub private repositories:

1. If accessing private repositories please ensure you have associated a **PUBLIC** GitHub ID with your RDK Central Account:

>>  [RDK Central Github Profile Setup for Private Repository Access](https://wiki.rdkcentral.com/spaces/CMF/pages/347342833/RDK+Central+Github+Profile+Setup+for+Private+Repository+Access)

2. Once step 1 is done you will receive a GitHub Invite to the **email account associated with your PUBLIC GitHub Account** that you associated with your rdkcentral account, make sure you click on the invite.
- if you haven't received the invite within 24 hours then please send a support request to support@rdkcentral.com
- check https://github.com/settings/emails to see what your public GitHub primary email and other accounts are configured as for the public account you are using
- make sure to check Junk/Spam/Filtered emails if you don't see it
- the invite remains valid for 7 days
3. Note you can also accept the invite from https://github.com/settings/organizations if you are having trouble finding it in your email

_Note GitHub invites will only have been issued if you are a member of Comcast GHEC or have raised a support ticket requesting access to a private repo._

_Valid GHEC users will automatically receive the invite and do not need to raise a support ticket unless they haven't received it within 24 hours._ 

_If you entered an invalid GHEC or Public GitHub user ID the invite will not be sent until it is corrected._

_Comcast/Sky users do automatically have a GHEC account, in this case if you specify your Comcast user id as your GitHub ID and you do not have a valid/active GHEC account you will not get an automatic invite._

_If you are a member of RDK Central Github Teams already you will not receive a second invite, in this scenario please raise a support ticket and we will add you to the required team(s)._

_Invites are not sent on Saturday/Sunday._



# Working with multiple GitHub Organisations and Accounts
If you are a member of more than one GitHub organisation and have different GitHub accounts for them, e.g. your a member of rdkcentral and an enterprise GitHub organisation, you can setup  your access as follows:


```
~/.gitconfig

[url "https://PUBLIC_ID@github.com/rdkcentral"]
    insteadOf = "https://github.com/rdkcentral"
    insteadOf = "ssh://github.com/rdkcentral"
    insteadOf = "ssh://git@github.com/rdkcentral"
    insteadOf = "git@github.com:rdkcentral"
    insteadOf = "github.com:rdkcentral"

[url "https://ENTERPRISE_ID@github.com/company"]
    insteadOf = "https://github.com/company"
    insteadOf = "ssh://github.com/company"
    insteadOf = "ssh://git@github.com/company"
    insteadOf = "git@github.com:company"
    insteadOf = "github.com:company"
```


```

~/.netrc

machine github.com 
    login PUBLIC_ID 
    password PUBLIC_ID_PAT
machine github.com 
    login ENTERPRISE_ID 
    password ENTERPRISE_ID_PAT

```

# PR flagged as not having accepted CLA agreement
If you encounter the following issue when creating PR's on rdkcentral where the system flags the PR as not having accepted the CLA agreement, even though the user has already accepted it and the recheck link does not resolve the issue.

Ensure your git user.email matches the email associated with your CLA agreement. If the issue persists, raise a CMF-SUPPORT ticket for help.

To avoid email mismatches, you can use separate .gitconfig files for different remotes.

Add the following to your main ~/.gitconfig:


```
[includeIf "hasconfig:remote.*.url:git@github.com:rdkcentral/**"]
    path = "~/.gitconfig_rdkcentral"
[includeIf "hasconfig:remote.*.url:https://github.com/rdkcentral/**"]
    path = "~/.gitconfig_rdkcentral"
[includeIf "hasconfig:remote.*.url:git@github.com:rdk-e/**"]
    path = "~/.gitconfig_rdke"
[includeIf "hasconfig:remote.*.url:https://github.com/rdk-e/**"]
    path = "~/.gitconfig_rdke"

```

Then, in each ~/.gitconfig_* file, set the appropriate user/email:

~/.gitconfig_rdkcentral:


```
[user]
    name = Your Name
    email = your-email-associated-with-public-github@domain.com

```
~/.gitconfig_rdke:

```
[user]
    name = Your Name
    email = your-comcast-email@comcast.com
```

