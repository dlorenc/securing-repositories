# Securing Repositories

This document contains a set of best practices for securing OSS repositories.
Each practice contains a set of steps to manually verify the
practice is followed.
We also hope to eventually develop and maintain a tool to handle automatic verification.

## Access Controls

SCM (software control management) systems provide a myriad of controls governing who has permissions to read or modify source code contained in the system.
Regularly audit the settings of your source code repositories to ensure that users only have access to permissions they truly need.
This section contains some things to consider when evaluating your permissions.

### 2 Factor Authentication

This is a must! If you haven't already, stop reading this right now and enable 2FA on your account.
Then, require 2FA for everyone with write permissions to your repository.
See the documentation for your SCM provider for instructions on how to do this.
[Github](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/requiring-two-factor-authentication-in-your-organization), [GitLab](https://docs.gitlab.com/ee/security/two_factor_authentication.html)

### Protected Branches

Settings like "Protected Branches" are available across many SCM providers, including [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html) and [GitHub](https://help.github.com/en/github/administering-a-repository/about-protected-branches).
Take advantage of these to further lockdown the branches of your project that are used for critical builds and releases.

### Required Pull/Merge Requests

If your workflow allows, many SCM providers allow you to require all changes to go through a pull request.
This stops users from pushing commits directly to shared branches, which can prevent accidents as well as increase security.

### Required Code Review

Building upon protected branches and pull/merge requests, you can also require code review before code is merged.
This can be important when working in regulated environments such as [CFR Part 11](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?CFRPart=11&showFR=1&subpartNode=21:1.0.1.1.8.3), but is also a general best practice.
Make sure you use the settings in your SCM provider to require that only the correct users are allowed to grant approvals to merge code.
Generally this can be restricted at the repository-level, but some providers also have systems to enforce this at a directory-level within a repository.

Additionally, make sure that your configuration requires another review if anything has changed since the last review.
GitHub calls this "Dismiss Stale Reviews".
GitLab calls this feature "Resetting Approvals on Push". Other systems may refer to it by other names.
Overall, this feature is used to prevent the code from being modified after it was looked at, but before it is submitted.

## Secure Releases

In addition to code, many SCM systems also provide mechanisms for tagging releases and basic artifact hosting. When using these features, there are several considerations to think about.

### Tags and Releases

Similar to branch protection, make sure to enable "Tag Protection" on any tags that signify important releases of your software.
This can help prevent users from accidentally or maliciously overwriting a tagged release to point elsewhere.

When cutting a release, publishing the digest and signatures of artifacts can allow your users to have greater confidence in the provenance of the software you release.
The [Debian wiki](https://wiki.debian.org/Creating%20signed%20GitHub%20releases) has some great resources on how to do this.

### Signing Commits

Similarly to signing artifacts, requiring contributors to sign their commits can increase confidence in the identity of the contributors to a piece of software.
If signing commits is too burdensome, using signed tags that correspond to releases is another helpful strategy.

### Security Policies

Once your repository has been configured with the above protections, you should also think about how you want to handle other types of vulnerabilities found in your software.
This is called a "Security Policy", and can inform your users on how you think about and manage security issues.
At a minimum, security policies should include information such as which versions of your software are supported and how to report security vulnerabilities.
These are typically placed at the root of your repository in a file called "security.md", although practices can vary across ecosystems.

Other information such as public keys used for signing releases and your security disclosure policy can go in this file as well.
