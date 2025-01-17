# GitHub integration

Snyk's GitHub integration allows you to continuously perform security scanning across all the integrated repositories, detect vulnerabilities in your open source components, and provide automated fixes.

Note that GitHub integrates _per user_ and ** **_**not**_** ** _per organization_. Setting up this integration means it will be used for _**all**_ the organizations associated with your account.

{% hint style="warning" %}
You cannot use the GitHub integration to import projects via the Snyk API with a Snyk Service Account: this is because the integration is associated with your user account and _**not**_ with the Snyk organization.\
&#x20;\
If you want to import projects via the API with a Snyk Service Account, use the [GitHub Enterprise integration](github-enterprise-integration.md).
{% endhint %}

## Setting up a GitHub Integration

1. Navigate to **Integrations > GitHub**.
2. Choose whether you'd like to give Snyk access to both public and private repositories or only to public repositories. \
   The GitHub authorization screen is displayed.&#x20;
3. In the GitHub authorization screen, click **Authorize Snyk** to provide Snyk with access to your repositories.
4. Select the repositories you want to import to Snyk and click **Add selected repositories** (upper right corner of the page).&#x20;
   * After you add the repositories, Snyk starts scanning the selected repositories for dependency files (that is, _package.json_, _pom.xml_, and so on) in the entire directory tree and imports them to Snyk as projects.
   * The imported projects appear in your **Projects** page and are continuously checked for vulnerabilities.

![](<../../../.gitbook/assets/which\_repos (3) (5) (9) (7) (18) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (27) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (31).jpg>)

## GitHub integration features

Once the integration is in place, you'll be able to use the following capabilities:

### • **Project level security reports**

Snyk produces advanced security reports that let you explore the vulnerabilities found in your repositories and fix them right away by opening a fix pull request directly in your repository, with the required upgrades or patches.

The example below presents a project-level security report.

![](<../../../.gitbook/assets/mceclip0-22- (2) (5) (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (29).png>)

### • **Project monitoring and automatic fix pull requests**

Snyk scans your projects on either a daily or a weekly basis. When new vulnerabilities are found, Snyk notifies you via email and opens automated pull requests with fixes for your repositories.&#x20;

The example below presents a fix pull request opened by Snyk.

![image7.png](../../../.gitbook/assets/uuid-6cfdaf0b-c349-468d-fe65-4f80bad110ea-en.png)

You can review and adjust the automatic fix pull request settings in the Snyk GitHub Integration settings page via <img src="../../../.gitbook/assets/cog_icon.png" alt="" data-size="line"> (Organization Settings) **>** **Integrations > Source control > GitHub**.

![](<../../../.gitbook/assets/mceclip4 (1) (2) (6) (7) (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (27).png>)

### • **** Commit Signing

All the commits in Snyk's PRs are done by [snyk-bot@snyk.io](mailto:snyk-bot@snyk.io) (a verified user on GitHub), and signed with a PGP key: all Snyk PRs appear as verified on GitHub, thus providing your developers with the confidence that the fix and upgrade PRs are generated by a trusted source.

{% hint style="info" %}
**Feature availability**

This feature is _not_ supported for brokered GitHub integrations.
{% endhint %}

### • **Pull request testing**

Snyk tests any newly created pull request in your repositories for security vulnerabilities and sends a status check to GitHub. This lets you see whether the pull request introduces new security issues, directly in GitHub.

The example below presents how Snyk pull request checks appear on the Pull Request page on GitHub.

![](<../../../.gitbook/assets/uuid-87113833-be79-dbe2-8860-a3f224d654c4-en (2) (2) (6) (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (28).png>)

You can review and adjust the pull request tests settings via the Snyk GitHub Integration settings page in <img src="../../../.gitbook/assets/cog_icon.png" alt="" data-size="line"> (Organization Settings)  **>** **Integrations > Source control > GitHub**.&#x20;

![](<../../../.gitbook/assets/mceclip5 (1).png>)

## Required permissions scope for the GitHub integration

### Non-Brokered GitHub Integrations

1. Operations that are triggered via the Snyk Web UI (for example, opening a Fix PR or retesting a project) are performed on behalf of the acting user. Therefore, a user who wants to perform this operation on GitHub via the Snyk UI must connect their GitHub account to Snyk, and have the required permissions scope for the repositories they want to perform these operations for. See the [Required permissions scope for repositories](github-integration.md#h\_01eefvj14p8b3depeffvyvdwzj) section for more details.
2. Operations that are not triggered via the Snyk Web UI, such as daily / weekly tests and automatic PRs (fix and upgrade), are performed on behalf of random Snyk organization members who have connected their GitHub accounts to Snyk and have the required permission scope for the repository.
3. For public repositories that are non-brokered, some operations (such as creating the PR) may occasionally be performed by [snyk-bot@snyk.io](mailto:snyk-bot@snyk.io).

{% hint style="info" %}
A Snyk organization administrator can [designate a specific GitHub account to use for opening fix and upgrade PRs](opening-fix-and-upgrade-pull-requests-from-a-fixed-github-account.md).&#x20;

Note that Snyk will continue to use a random Snyk organization member’s GitHub account to perform all the other operations, therefore using this feature does not eliminate the need to connect users' GitHub accounts to Snyk.
{% endhint %}

### Brokered GitHub Integrations

All the operations, both those that are triggered via the Snyk Web UI and the automatic operations, are performed on behalf of a GitHub service account that has its token configured with the Broker.&#x20;

The table below presents a summary of the required access scopes for the configured token.

| **Action**                                          | **Purpose**                                                                                                                                          | **Required permissions in GitHub** |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Daily / weekly tests                                | Used for reading manifest files in private repos                                                                                                     | _repo (all)_                       |
| Manual fix pull requests (triggered by the user)    | Used for creating fix PRs in the monitored repos                                                                                                     | _repo (all)_                       |
| Automatic fix and upgrade pull requests             | Used for creating fix or upgrade PRs in the monitored repos                                                                                          | _repo (all)_                       |
| Snyk tests on pull requests                         | Used for sending pull request status checks whenever a new PR is created or an existing PR is updated                                                | _repo (all)_                       |
| Importing new projects to Snyk                      | Used for presenting a list of all the available repos in the GitHub org in the **Add Projects** screen (import popup)                                | _admin:read:org, repo (all)_       |
| Snyk tests on pull requests - initial configuration | Used for adding Snyk's webhooks to the imported repos, so that Snyk is informed when pull requests are created or updated and can then trigger scans | _admin:repo\_hooks (read & write)_ |

## Required permission scope for repositories <a href="#h_01eefvj14p8b3depeffvyvdwzj" id="h_01eefvj14p8b3depeffvyvdwzj"></a>

For Snyk to be able to perform the required operation on monitor repositories (that is, reading manifest files on a frequent basis and opening fix or upgrade PRs), the accounts that are connected to Snyk (either directly or via Broker) require the following access to the repositories:

| **Action**                                          | **Why?**                                                                                                                                              | **Required permissions on the repository** |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| Daily / weekly tests                                | For reading manifest files in private repos                                                                                                           | _Read_ or higher                           |
| Snyk tests on pull requests                         | For sending pull request status checks whenever a new PR is created / an existing PR is updated                                                       | _Write_ or higher                          |
| Opening fix and upgrade pull requests               | For creating fix / upgrade PRs in the monitored repos                                                                                                 | _Write_ or higher                          |
| Snyk tests on pull requests - initial configuration | For adding Snyk's webhooks to the imported repos, so Snyk will be informed whenever pull requests are created or updated and be able to trigger scans | _Admin_                                    |

### **1. Setting an account to open Snyk PRs**

Snyk lets you designate a specific GitHub account to open fix and upgrade pull requests.&#x20;

Note that the configured account is only used for _opening_ PRs. All the other operations are still  performed on behalf of a random Snyk organization member who has connected their GitHub accounts to Snyk.

To use this feature, follow the steps below:

1. Navigate to the GitHub Integrations settings page in the Snyk Web UI, via <img src="../../../.gitbook/assets/cog_icon.png" alt="" data-size="line"> (Organization Settings)  **>** **Integrations > Source control > GitHub.**
2. In the **Set an account to open Snyk PRs** section, set the toggle to **Enabled**.
3. In the right side of the section, follow the instructions to **Create a personal access token in GitHub**.
4. In the **Personal access token** field, paste the newly generated token to Snyk so it can be used to perform operations on GitHub (that is, opening Fix PRs, and so on).

![](../../../.gitbook/assets/set\_account\_for\_snyk\_prs.png)

{% hint style="info" %}
Make sure that the GitHub account that you designate to open Snyk PRs has at least **write** level permissions (or higher) for the repos you want to monitor with Snyk.

See [repository permission levels on GitHub](github-integration.md#required-permissions-scope-for-the-github-integration) for more information.
{% endhint %}

### **2. Assigning pull requests to users**  <a href="#pr-assignment" id="pr-assignment"></a>

Snyk can automatically assign the pull requests it creates to ensure that they are handled by the right team members.

Auto-assign for PRs can be enabled for the GitHub integration (and all projects imported via GitHub), or on a per-project basis.&#x20;

{% hint style="info" %}
**Feature availability**

The Auto-assign PRs feature is only supported for private repositories.
{% endhint %}

Users can either be manually specified (and all will be assigned) or automatically selected based on the last commit user account.

#### • **Enable Auto-assign for all projects in the Github integration:**&#x20;

Navigate to the Github integration settings via <img src="../../../.gitbook/assets/cog_icon.png" alt="" data-size="line"> (Organization Settings)  **>** **Integrations > Source control > GitHub** and configure the following options:&#x20;

![](../../../.gitbook/assets/code-assignees-contribs.png)

• **Enable Auto-assign for a single project**

To configure the Auto-assign settings for a specific project**:**

1. In the **Projects** tab for your organization, select and expand the relevant private repository, select a target, and click the **Settings** cog.\
   &#x20;<img src="../../../.gitbook/assets/image (56) (2) (3) (3) (3) (3) (4) (5) (5) (5) (5) (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (10).png" alt="" data-size="original">\
   The project page opens.&#x20;
2. In the project page, to apply unique settings for that specific project, select the **Settings** tab in the upper right, and the **Github integration** __ option in the left sidebar.
3. Select, customize, and confirm the relevant options in the sections:

![](../../../.gitbook/assets/code-assigness-project.png)

## Disconnecting the GitHub integration

Snyk’s GitHub SCM integration leverages the oAuth app integration. If you integrated GitHub without using the Broker, you can simply disconnect it by following these steps:

1. Log in to the GitHub account that you used to create the integration.
2. Navigate to your GitHub account settings and select the **Applications** option in the left sidebar.
3. Select the **Authorized OAuth Apps** tab.\
   You can also reach the [Authorized OAuth Apps tab directly](https://github.com/settings/applications).
4. Find the **Snyk** entry, click the 3 dots on the right, and select **Revoke**.

![](../../../.gitbook/assets/disconnect\_github\_19-06-2022.png)

Revoking this access effectively disconnects Snyk’s access to that GitHub account. Existing imported snapshots will persist in Snyk and continue to rescan based on the existing snapshots until deleted. Snyk will no longer be able to import new projects from the GitHub integration and will no longer re-scan on new code merges.

Additionally, you must confirm that Snyk is not enabled on any existing **Branch protection rules**:  From the main page of your GitHub repository, navigate to **Settings > Branches > Branch protection rules,** and make sure there are no **Status checks found in the last week for this repository.**

## GitHub badges

Once you’re vulnerability free, you can put a badge on your README page to let the world know that your package has no known security holes. This shows your users that you care about security, and tells them that they should care too.

### Repository badges

The green badge indicates that there are no vulnerabilities.

![](../../../.gitbook/assets/uuid-cb438aa4-226e-2109-f901-c59ca233732e-en.png)

The red badge indicates how many vulnerabilities have been found.

![](../../../.gitbook/assets/uuid-96d6b4d1-afb7-a2bd-093d-eaa96e2ac2c1-en.png)

To show a badge for a given Node.js, Ruby or Java GitHub repository, copy the relevant snippet below and replace “{username}/{repo}” with the GitHub username and repo you want to test.

**HTML:**

`<*a href="https://snyk.io/test/github/{username}/{repo}">`![image2.png](../../../.gitbook/assets/uuid-2678b218-659f-61c6-8e14-57964dfb2bc6-en.png)

**Markdown:**

`[![Known Vulnerabilities](https://snyk.io/test/github/{username}/{repo}/badge.svg)](https://snyk.io/test/github/{username}/{repo})`

The badge indicates the vulnerability state of the latest commit on the master branch. To show the vulnerability state of a specific branch, release or tag, simply add its name after the repo name in the URL.

For example, to show a badge for the 4.x branch of the express repo, use the URL [https://snyk.io/test/github/expressjs/express/4.x/badge.svg](https://snyk.io/test/github/expressjs/express/4.x/badge.svg).

**Styles**

To change the style of the badge, you can add the following query parameters after `badge.svg`:

![](../../../.gitbook/assets/uuid-cb438aa4-226e-2109-f901-c59ca233732e-en.png)

`?style=flat-square`

![](../../../.gitbook/assets/uuid-cb438aa4-226e-2109-f901-c59ca233732e-en.png)

`style=plastic`

### **npm badges**

To show a badge for a given npm package, copy the relevant snippet below, and replace “{name}” with the name of your package.

**HTML**

`<*img src="https://snyk.io/test/npm/{name}/badge.svg" alt="Known Vulnerabilities" data-canonical-src="https://snyk.io/test/npm/{name}" style="max-width:100%;"/>`

**Markdown**

`[![Known Vulnerabilities](https://snyk.io/test/npm/{name}/badge.svg)](https://snyk.io/test/npm/{name})`

The badge will reflect the vulnerability state of the latest version of this package. To show the vulnerability state of a specific package, you can specify the specific version in the URL.

For example, to test version 1.2.3 of package name, use the URL [https://snyk.io/test/npm/name/1.2.3/badge.svg](https://snyk.io/test/npm/name/1.2.3/badge.svg).

### **Private packages and repos**

Badges currently only work for public npm packages and GitHub repositories, and will fail if pointed at a private repository. To continuously watch for vulnerabilities in your GitHub repositories, both public and private, consider integrating them with Snyk.

### **Custom manifest file locations**

By default, the badge will test against the first [valid manifest file](../../../products/snyk-open-source/language-and-package-manager-support/) it detects in the root of your project.

If your manifest file is in another location other than the root of the repository, or if you have multiple manifest files that you would like to show a badge for, you can pass a targetFile query string parameter to direct the badge to test against another supported manifest file.

**HTML:**

`<*a href="https://snyk.io/test/github/{username}/{repo}">`

**Markdown:**

`[![Known Vulnerabilities](https://snyk.io/test/github/{username}/{repo}/badge.svg?targetFile={path-to-target-file})](https://snyk.io/test/github/{username}/{repo})`
