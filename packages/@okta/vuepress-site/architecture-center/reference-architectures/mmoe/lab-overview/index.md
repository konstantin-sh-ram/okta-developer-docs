---
title: Manage multiple Okta environments Lab overview
---

# Manage multiple Okta environments: Lab overview

Before incorporating Terraform into a company's continuous deployment architecture, there isn't a way to create, rebuild, or update an Okta environment consistently. Nor is it easy to determine if one environment has changed - for example, a group updated, a new autentication policy - and to synchronize the others with it. This is known as analysing drift between environments.

This lab is intended for admins with Okta experience who have multiple orgs. It assumes that you have previous knowledge of Okta, but not necessarily about Terraform Cloud. For more information on:

* Okta and IAM, see [Identity and Access Management (IAM) overview](https://developer.okta.com/docs/concepts/iam-overview/).
* Terraform Cloud, see [What is Terraform Cloud?](https://developer.hashicorp.com/terraform/cloud-docs).

The tutorials in this lab are designed to run sequentially, with each tutorial starting with the results on the one before:

1. In [How to configure Terraform Cloud](/architecture-center/reference-architectures/mmoe/lab-1-configure-terraform-cloud), you create a free Terraform Cloud account, connect it to your Github account and development Okta org. Then, you configure it to react to changes in your Github repo.
1. In [How to create resources for your environment](/architecture-center/reference-architectures/mmoe/lab-2-create-resources), you use Terraform to create the resources required to make your org functional.
1. In [How to change an object in that environment](/architecture-center/reference-architectures/mmoe/lab-3-rename-a-group), you use Terraform to update the name of an existing group in the new org.
1. In [How to move objects between environments](/architecture-center/reference-architectures/mmoe/lab-4-deploy-changes-to-production), you deploy your changes from the development environment to a separate production environment.
1. In [How to detect drift between environments and correct it](/architecture-center/reference-architectures/mmoe/lab-5-detect-drift), you update a resource in production outside of Terraform, and then run a Terraform plan to detect if an asset is changed.
1. In [How to schedule drift detection daily](/architecture-center/reference-architectures/mmoe/lab-6-synchronize-environments-daily), you create a workflow to automatically run a speculative plan daily in production to detect drift.

## Prerequisites

You will need to set up the following to complete the tutorials in this lab.

## Applications

This lab uses [Okta CLI](https://cli.okta.com/) to create Okta developer accounts quickly.

> **Tip:** Before running Okta CLI, consider adding the directory path to its executable to your `PATH` environment variable.

## Two Okta developer accounts

This lab requires two Okta developer accounts, one to represent a development environment, and one to act as the live environment. Both need to be running **Okta Identity Engine**.

> **Tip:** If you are using existing accounts and want to check that the org is running Okta Identity Engine rather than Okta Classic Engine, check the footer on any page of the Admin Console for that org. The version number is appended with **E** for Identity Engine orgs and **C** for Classic Engine orgs.

Okta CLI is the quickest way to create an Okta org, so we recommend using it to create both new orgs. Alternatively, you can manually sign up instead.

1. Open your terminal.
2. Run `okta register`, and enter your First name, Last name, Email address, and Country.
3. Click or tap **Activate** in the account activation email that is sent to the email address that you provided.
4. After your domain is registered, look for output similar to this:

   ```txt
   Your Okta Domain: https://dev-xxxxxxx.okta.com
   To set your password open this link:
   https://dev-xxxxxxx.okta.com/welcome/xrqyNKPCZcvxL1ouKUoh
   ```

5. Set the password for your Okta developer org by opening the link and following the instructions.
6. After you enter your password, your Okta domain is returned, similar to the following. Make note of it. This is your development domain.

   ```txt
   New Okta Account created!
   Your Okta Domain: https://dev-xxxxxxx.okta.com
   ```

7. Repeat steps 1 - 6 to create a second Okta domain. This is your production domain.

> **Tip:** If you don't receive the confirmation email sent as part of the creation process, check your spam filters for an email from `noreply@okta.com`.

## Okta Workflows

Tutorial 6 requires your production org to have [Okta Workflows](https://www.okta.com/platform/workflows/) enabled. To determine if it's enabled:

1. Open the Admin Console for your production org.
2. Choose **Workflow** from the menu.
3. If there is no Workflows console item under Workflow, then Workflows needs to be enabled for the Okta account that you are using. Reach out to your customer account team to have this done.

## A Slack channel

Tutorial 6 uses Slack as the target for its workflow notification messages. To create a free trial Slack instance and channel:

1. Visit [slack.com](https://slack.com), and click **TRY FOR FREE**.
2. Supply your email address and click **Continue**.
3. Enter the 6-digit code that's sent to your email address.
4. Click **Create a Workspace** to create a Slack workspace.
5. Specify your company name, identify other team members to invite to the workspace (if any), and enter a short name used as the name of the channel. For example, _Terraform Drift_.

## A working Terraform repository

You need to create a copy of an example Terraform script in your GitHub repository.

1. Open `https://github.com/oktadev/okta-reference-multi-org-terraform-example` in a browser.
2. Click **Fork** to create a copy for your project in GitHub.
3. In the **Create a new fork** page
   1. **Clear** the **Copy the prod branch only** checkbox.
   1. Click **Create fork**.

The repository you are forking has two branches: prod and preview. The preview branch should be populated and is the one to use in tutorials 2 and 3. The prod branch should be empty. In tutorial 4 onwards, promote your code to prod and then use the prod branch for the remaining exercises.

## Terraform Cloud account

Terraform has two offerings for building your automation. The first is available for free through a Command Line Interface (CLI). Another option is through their paid SaaS offering, [Terraform Cloud](https://cloud.hashicorp.com/products/terraform), which is free to try. In this lab, you use an instance of Terraform Cloud. Tutorial 1 covers how to create and configure your account.

## Values and variables

Connect Terraform to Okta using the following values:

* `${OKTA_DOMAIN}` is the full URL of your Okta developer org.
   For example, `https://dev-133337.okta.com`.
* `${OKTA_DOMAIN_NAME}` is the subdomain of your Okta developer org.
   For example, `dev-133337`.

To find these settings in the Admin Console, see [Configuration Settings](/docs/guides/oie-embedded-common-download-setup-app/java/main/#configuration-settings).