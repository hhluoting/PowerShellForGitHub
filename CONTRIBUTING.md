# PowerShellForGitHub PowerShell Module
## Contributing

Looking to help out?  You've come to the right place.  We'd love your help in making this the best
way to automate GitHub repos.

Looking for information on how to use this module?  Head on over to [README.md](README.md).

----------
#### Table of Contents

*   [Overview](#overview)
*   [Maintainers](#maintainers)
*   [Feedback](#feedback)
    *   [Bugs](#bugs)
    *   [Suggestions](#suggestions)
    *   [Questions](#questions)
*   [Static Analysis](#static-analysis)
*   [Module Manifest](#module-manifest)
*   [Logging](#logging)
*   [PowerShell Version](#powershell-version)
*   [Coding Guidelines](#coding-guidelines)
*   [Adding New Configuration Properties](#adding-new-configuration-properties)
*   [Code Comments](#code-comments)
*   [Debugging Tips](#debugging-tips)
*   [Testing](#testing)
    *   [Installing Pester](#installing-pester)
    *   [Configuring Your Environment](#configuring-your-environment)
    *   [Running the Tests](#running-the-tests)
    *   [Automated Tests](#automated-tests)
    *   [New Test Guidelines](#new-test-guidelines)
*   [Releasing](#releasing)
    *   [Updating the CHANGELOG](#updating-the-changelog)
    *   [Adding a New Tag](#adding-a-new-tag)
    *   [Publish a Signed Update to PowerShell Gallery](#publish-a-signed-update-to-PowerShellGallery)
*   [Contributors](#contributors)
*   [Legal and Licensing](#legal-and-licensing)

----------

## Overview

We're excited that _you're_ excited about this project, and would welcome your contributions to help
it grow.  There are many different ways that you can contribute:

 1. Submit a [bug report](#bugs).
 2. Verify existing fixes for bugs.
 3. Submit your own fixes for a bug. Before submitting, please make sure you have:
   * Performed code reviews of your own
   * Updated the [test cases](#testing) if needed
   * Run the [test cases](#testing) to ensure no feature breaks or test breaks
   * Added the [test cases](#testing) for new code
   * Ensured that the code is free of [static analysis](#static-analysis) issues
 4. Submit a feature request.
 5. Help answer [questions](https://github.com/PowerShell/PowerShellForGitHub/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aopen%20label%3Aquestion).
 6. Write new [test cases](#testing).
 7. Tell others about the project.
 8. Tell the developers how much you appreciate the product!

You might also read these two blog posts about contributing code:
 * [Open Source Contribution Etiquette](http://tirania.org/blog/archive/2010/Dec-31.html) by Miguel de Icaza
 * [Don't "Push" Your Pull Requests](http://www.igvita.com/2011/12/19/dont-push-your-pull-requests/) by Ilya Grigorik.

Before submitting a feature or substantial code contribution, please discuss it with the
PowerShellForGitHub team via [Issues](https://github.com/PowerShell/PowerShellForGitHub/issues), and ensure it
follows the product roadmap. Note that all code submissions will be rigorously reviewed by the
PowerShellForGitHub Team. Only those that meet a high bar for both quality and roadmap fit will be merged
into the source.

## Maintainers

PowerShellForGitHub is maintained by:

- **[@HowardWolosky](http://github.com/HowardWolosky)**

As this module is a production dependency for Microsoft, we have a couple workflow restrictions:

- Anyone with commit rights can merge Pull Requests provided that there is a :+1: from one of
  the members above.
- Releases are performed by a member above so that we can ensure Microsoft internal processes
  remain up to date with the latest and that there are no regressions.

## Feedback

All issue types are tracked on the project's [Issues]( https://github.com/PowerShell/PowerShellForGitHub/issues)
page.

In all cases, make sure to search the list of issues before opening a new one.
Duplicate issues will be closed.

### Bugs

For a great primer on how to submit a great bug report, we recommend that you read:
[Painless Bug Tracking](http://www.joelonsoftware.com/articles/fog0000000029.html).

To report a bug, please include as much information as possible, namely:

* The version of the module (located in `PowerShellForGitHub.psd1`)
* Your OS version
* Your version of PowerShell (`$PSVersionTable.PSVersion`)
* As much information as possible to reproduce the problem.
* If possible, logs from your execution of the task that exhibit the erroneous behavior
* The behavior you expect to see

Please also mark your issue with the 'bug' label.

### Suggestions

We welcome your suggestions for enhancements to the extension.
To ensure that we can integrate your suggestions effectively, try to be as detailed as possible
and include:

* What you want to achieve / what is the problem that you want to address.
* What is your approach for solving the problem.
* If applicable, a user scenario of the feature / enhancement in action.

Please also mark your issue with the 'suggestion' label.

### Questions

If you've read through all of the documentation, checked the Wiki, and the PowerShell help for
the command you're using still isn't enough, then please open an issue with the `question`
label and include:

* What you want to achieve / what is the problem that you want to address.
* What have you tried so far.

----------

## Static Analysis

This project leverages the [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer/)
PowerShell module for static analysis.

It is expected that this module shall remain "clean" from the perspective of that module.

If you have never installed PSScriptAnalyzer, do this from an Administrator PowerShell console window:

```powershell
Install-Module -Name PSScriptAnalyzer
```

In the future, before running it, make sure it's up-to-date (run this from an Administrator
PowerShell console window):

```powershell
Update-Module -Name PSScriptAnalyzer
```

Once it's installed (or updated), from the root of your enlistment simply call

```powershell
Invoke-ScriptAnalyzer -Path .\ -Recurse
```

That should return with no output.  If you see any output when calling that command,
either fix the issues that it calls out, or add a `[Diagnostics.CodeAnalysis.SuppressMessageAttribute()]`
with a justification explaining why it's ok to suppress that rule within that part of the script.
Refer to the [PSScriptAnalyzer documentation](https://github.com/PowerShell/PSScriptAnalyzer/) for
more information on how to use that attribute, or look at other existing examples within this module.

----------

### Module Manifest

This is a manifested PowerShell module, and the manifest can be found here:

    PowerShellForGitHub.psd1

If you add any new modules/files to this module, be sure to update the manifest as well.
New modules should be added to `NestedModules`, and any new functions or aliases that
should be exported need to be added to the corresponding `FunctionsToExport` or
`AliasesToExport` section.  Please keep all entries to those sections in **alphabetical order**.

----------

### Logging

Instead of using the built-in `Write-*` methods (`Write-Host`, `Write-Warning`, etc...),
please use

```powershell
Write-Log
```

which is implemented in Helpers.ps1.  It will take care of formatting your content in a
consistent manner, as well ensure that the content is logged to a file (if configured to do so
by the user).

----------

### PowerShell Version

This module must be able to run on PowerShell version 4.  It is permitted to add functionality
that requires a higher version of PowerShell, but only if there is a fallback implementation
that accomplishes the same thing in a PowerShell version 4 compatible way, and the path choice
is controlled by a PowerShell version check.

For an example of this, see `Write-Log` in `Helpers.ps1` which uses `Write-Information`
for `Informational` messages on v5+ and falls back to `Write-Host` for earlier versions:

```powershell
if ($PSVersionTable.PSVersion.Major -ge 5)
{
    Write-Information $ConsoleMessage -InformationAction Continue
}
else
{
    Write-Host $ConsoleMessage
}
```

----------

### Coding Guidelines

As a general rule, our coding convention is to follow the style of the surrounding code.
Avoid reformatting any code when submitting a PR as it obscures the functional changes of your change.

A basic rule of formatting is to use "Visual Studio defaults".
Here are some general guidelines

* No tabs, indent 4 spaces.
* Braces usually go on their own line,
  with the exception of single line statements that are properly indented.
* Use `camelCase` for instance fields, `PascalCase` for function and parameter names
* Avoid the creation of `script` scoped variables unless absolutely necessary.
  If referencing one, be sure to explicitly reference it by scope.
* Don't use globals.  If you want to add module configuration, [add a new property instead](#adding-new-configuration-properties).
* Avoid more than one blank empty line.
* Always use a blank line following a closing bracket `}` unless the next line itself is a closing bracket.
* Add full [Comment Based Help](https://technet.microsoft.com/en-us/library/hh847834.aspx) for all
  methods added, whether internal-only or external.  The act of writing this documentation may help
  you better design your function.
* File encoding should be ASCII (preferred) or UTF8 (with BOM) if absolutely necessary.
* We try to adhere to the [PoshCode Best Practices](https://github.com/PoshCode/PowerShellPracticeAndStyle/tree/master/Best%20Practices)
  and [DSCResources Style Guidelines](https://github.com/PowerShell/DscResources/blob/master/StyleGuidelines.md)
  and think that you should too.
* We try to limit lines to 100 characters to limit the amount of horizontal scrolling needed when
  reviewing/maintaining code.  There are of course exceptions, but this is generally an enforced
  preference.  The [Visual Studio Productivity Power Tools](https://visualstudiogallery.msdn.microsoft.com/34ebc6a2-2777-421d-8914-e29c1dfa7f5d)
  extension has a "Column Guides" feature that makes it easy to add a Guideline in column 100
  to make it really obvious when coding.

----------

## Adding New Configuration Properties

If you want to add a new configuration value to the module, you must modify the following:
 * In `Import-GitHubConfiguration`, update `$config` to declare the new property along with
   its default value, being sure that PowerShell will understand what its type is. Properties
   should be alphabetical.
 * Update `Get-GitHubConfiguration` and add the new property name to the `ValidateSet` list
   so that tab-completion and documentation gets auto-updated. You shouldn't have to add anything
   to the body of the method. Property names should be alphabetical.
 * Add a new explicit parameter to `Set-GitHubConfiguration` to receive the property, along with
   updating the CBH (Comment Based Help) by adding a new `.PARAMETER` entry. You shouldn't
   have to add anything to the body of the method. Parameters should be alphabetical save for the
   `SessionOnly` switch, which should be last.

----------

### Code comments

It's strongly encouraged to add comments when you are making changes to the code and tests,
especially when the changes are not trivial or may raise confusion.
Make sure the added comments are accurate and easy to understand.
Good code comments should improve readability of the code, and make it much more maintainable.

That being said, some of the best code you can write is self-commenting.  By refactoring your code
into small, well-named functions that concisely describe their purpose, it's possible to write
code that reads clearly while requiring minimal comments to understand what it's doing.

----------

### Debugging Tips

You may find it useful to configure the module to log the body of all REST requests during
development of a new feature, to make it easier to see exactly what is being sent to GitHub.

```powershell
Set-GitHubConfiguration -LogRequestBody
```

----------

### Testing
[![Build status](https://ci.appveyor.com/api/projects/status/vsfq8kxo2et2dn7i?svg=true
)](https://ci.appveyor.com/project/HowardWolosky/powershellforgithub)

#### Installing Pester
This module supports testing using the [Pester UT framework](https://github.com/pester/Pester).

To install it:

```powershell
Install-Module -Name Pester
```

#### Configuring Your Environment
The tests intentionally do not mock out interaction with the real GitHub API, as we want to know
when our interaction with the API has been broken.  That means that to execute the tests, you will
need Administrator privelege for an account.  For our purposes, we have a "test" account that our
team uses for having the tests [run automated](#automated-tests).  For you to run the tests locally,
you must make a couple changes:

 1. Choose if you'll be executing the tests on your own pesonal account or your own test account
    (the tests should be non-destructive, but ... hey ... we are developing code here, mistakes happen.)
 2. Update your local copy of [tests/config/Settings.ps1](./tests/config/Settings.ps1) to note
    the `OwerName` and `OrganizationName` that the tests will be running under.
    > While you can certainly check-in this file to your own fork, please DO NOT include your
    > changes as part of any pull request that you may make.  The `.gitignore` file tries
    > to help prevent that.
 3. Run `Set-GitHubAuthentication` to ensure that it is configured with an administrator-level
    Access Token for the specified owner/organization.
    > Unfortunately, you cannot use `-SessionOnly` with `Set-GitHubAuthentication` when testing,
    > as Pester works by making new sessions for every test.  That means that it must be "globally"
    > configured with that access token for the duration of the Pester test execution.

#### Running the Tests
Tests can be run either from the project root directory or from the `Tests` subfolder.
Navigate to the correct folder and simply run:

```powershell
Invoke-Pester
```

Make sure you have previously configured your Access Token via `Set-GitHubAuthentication`.
Please keep in mind some tests may fail on your machine, as they test private items (e.g. secret teams) which your key won't have access to.

Pester can also be used to test code-coverage, like so:

```powershell
Invoke-Pester -CodeCoverage "$root\GitHubLabels.ps1" -TestName "*"
```

This command tells Pester to check the `GitHubLabels.ps1` file for code-coverage.
The `-TestName` parameter tells Pester to run any `Describe` blocks with a `Name` like
`"*"` (which in this case, is every test, but can be made more specific).

The code-coverage object can be captured and interacted with, like so:

```powershell
$cc = (Invoke-Pester -CodeCoverage "$root\GitHubLabels.ps1" -TestName "*" -PassThru -Quiet).CodeCoverage
```

There are many more nuances to code-coverage, see
[its documentation](https://github.com/pester/Pester/wiki/Code-Coverage) for more details.

#### Automated Tests
[![Build status](https://ci.appveyor.com/api/projects/status/vsfq8kxo2et2dn7i?svg=true
)](https://ci.appveyor.com/project/HowardWolosky/powershellforgithub)

These test are configured to automatically execute upon any update to the `master` branch
on the `HowardWolosky` fork of `PowerShellForGitHub`.  (At this time, we don't have the proper
permissions to have AppVeyor execute the tests on the main instance.)  This setup thus depends
on the `HowardWolosky` fork always being kept up-to-date with the `PowerShell` master.

The [AppVeyor project](https://ci.appveyor.com/project/HowardWolosky/powershellforgithub) has
been [configured](./appveyor.yml) to execute the tests against a test GitHub account (for the user
`PowerShellForGitHubTeam`, and the org `PowerShellForGitHubTeamTestOrg`).

Additionally, the Access Token with administrator access for that account has been encrypted on
AppVeyor and mentioned in the [config file](./appveyor.yml).  That Access Token is not accessible
by anyone else.  To run the tests locally with your own account, see
[configuring-your-environment](#configuring-your-environment).

#### New Test Guidelines
Your tests should have NO dependencies on an account being set up in a specific way.  They should
get the configured account set up in the appropriate state that it can then test/verify.  In this
way, anyone should be able to run the tests from their own machine/account.

Use a new GUID for any object that you have to create (repository, label, team name, etc...) to avoid
any possible name collisions with existing objects on the executing user's accounts.

----------

### Releasing

If you are a maintainer:

Ensure that the version number of the module is updated with every pull request that is being
accepted.

This project follows [semantic versioning](http://semver.org/) in the following way:

    <major>.<minor>.<patch>

Where:
* `<major>` - Changes only with _significant_ updates.
* `<minor>` - If this is a feature update, increment by one and be sure to reset `<patch>` to 0.
* `<patch>` - If this is a bug fix, leave `<minor>` alone and increment this by one.

When new code changes are checked in to the repo, the module must be signed and published by
Microsoft to [PowerShell Gallery](https://www.powershellgallery.com/packages/PowerShellForGitHub).

Once the new version has been pulled into master, there are additional tasks to perform:

  * Ensure that [all tests pass](#testing) and that the module is still [static analysis clean](#static-analysis)
  * Update [CHANGELOG.md](./CHANGELOG.md)
  * Add a tag for that version to the repo
  * Publish a _signed_ update to [PowerShellGallery](https://www.powershellgallery.com/packages/PowerShellForGitHub/).

#### Updating the CHANGELOG
To update [CHANGELOG.md](./CHANGELOG.md), just duplicate the previous section and update it to be
relevant for the new release.  Be sure to update all of the sections:
  * The version number
  * The hard path to the change (we'll get that path working in a moment)
  * The release date
  * A brief list of all the changes (use a `-` for the bullet point if it's fixing a bug, or a `+` for a feature)
  * The link to the pull request (pr) (so that the discussion on the change can be easily reviewed) and the changelist (cl)
  * The author (and a link to their profile)
  * If it's a new contributor, also add them to the [Contributors](#contributors) list below.

Then get a new pull request out for that change to CHANGELOG.

#### Adding a New Tag
To add a new tag:
   1. Make sure that you're in a clone of the actual repo and not your own private fork.
   2. Make sure that you have checked out `master` and that it's fully up-to-date
   3. Run `git tag -a '<version number>'`
   4. In the pop-up editor, give a one-line summary of the change (that you possibly already wrote for the CHANGELOG)
   5. Save and close the editor
   6. Run `git push --tags` to upload the new tag you just created

If you want to make sure you get these tags on any other forks/clients, you can run
`git fetch origin --tags` or `git fetch upstream --tags`, or whatever you've named the source to be.

#### Publish a Signed Update to PowerShellGallery
The final steps is getting the module updated on
[PowerShellGallery](https://www.powershellgallery.com/packages/PowerShellForGitHub/)
so that it is easy for users to adopt the changes.

The module files should be signed by Microsoft prior to publishing.
Instructions for signing the files and for then publishing the update to PowerShellGallery
can be found in the [internal Microsoft repo for this project](https://microsoft.visualstudio.com/Apps/_git/eng.powershellforgithub).

----------

### Contributors

Thank you to all of our contributors, no matter how big or small the contribution:

- **[Howard Wolosky (@HowardWolosky)](http://github.com/HowardWolosky)**
- **[Karol Kaczmarek (@KarolKaczmarek)](https://github.com/KarolKaczmarek)**
- **[Josh Rolstad (@jrolstad)](https://github.com/jrolstad)**
- **[Zachary Alexander (@zjalexander)](http://github.com/zjalexander)**
- **[Andrew Dahl (@aedahl)](http://github.com/aedahl)**
- **[Pepe Rivera (@joseartrivera)](https://github.com/joseartrivera)**
- **[Ethan Gottlieb (@etgottli)](https://github.com/etgottli)**
- **[Fran&ccedil;ois-Xavier Cat (@lazywinadmin)](https://github.com/lazywinadmin)**
- **[Cliff Chapman (@Cellivar)](https://github.com/Cellivar)**

----------

### Legal and Licensing

PowerShellForGitHub is licensed under the [MIT license](..\LICENSE).

You will need to complete a Contributor License Agreement (CLA) for any code submissions.
Briefly, this agreement testifies that you are granting us permission to use the submitted change
according to the terms of the project's license, and that the work being submitted is under
appropriate copyright. You only need to do this once.

When you submit a pull request, [@msftclas](https://github.com/msftclas) will automatically
determine whether you need to sign a CLA, comment on the PR and label it appropriately.
If you do need to sign a CLA, please visit https://cla.microsoft.com and follow the steps.
