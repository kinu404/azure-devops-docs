---
title: Get started with Copilot code review for pull requests
titleSuffix: Azure Repos
description: Learn how to enable and configure GitHub Copilot code review for pull requests in Azure Repos at the organization, repository, and user levels.
ms.service: azure-devops-repos
ms.subservice: azure-devops-repos-git
ms.topic: how-to
ai-usage: ai-assisted
ms.date: 07/09/2026
ms.author: chcomley
author: chcomley
---

# Get started with Copilot code review for pull requests

[!INCLUDE [version-eq-azure-devops](../../includes/version-eq-azure-devops.md)]

> [!IMPORTANT]
> This feature is in limited public **preview**.
>
> Functionality might change or be removed without notice. Preview features have no Service Level Agreement (SLA) and limited support.

Use GitHub Copilot to review pull requests in Azure Repos. Copilot acts as an automated reviewer that posts comments and suggestions on changed code, so you get feedback before a human reviewer signs off.

To use the feature, a Project Collection Administrator turns it on for the organization, a repository owner turns it on for each repository, and individual users opt in through Preview features (unless the administrator enables the preview for everyone).

## Prerequisites

| Category | Requirements |
|--|--|
| **Organization** | An [organization in Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137). |
| **Repository** | A Git repository in Azure Repos. TFVC isn't supported. |
| **Organization permissions** | **Project Collection Administrator** to enable the feature at the organization level. |
| **Repository permissions** | Repository owner or administrator to enable the feature for a repository. |
| **Billing** | An Azure subscription linked to your Azure DevOps organization. Copilot code review usage is billed through Azure Cost Management. For details, see [Billing](#billing). |

## Enable Copilot code review at the organization level

A Project Collection Administrator must enable Copilot code review for the organization before repository owners can turn it on for individual repositories.

1. Sign in to your Azure DevOps organization (`https://dev.azure.com/{yourorganization}`).
1. Select **Organization settings** > **Repos** > **Repositories**.
1. Under **GitHub Copilot code review**, toggle **Allow repositories in this organization to use Copilot code review** to **On**.

   :::image type="content" source="media/copilot-code-reviews/organization-level-preview-feature.png" alt-text="Organization settings page with the Allow repositories to use Copilot code review toggle set to On.":::

## Enable Copilot code review at the repository level

After organization-level access is enabled, a repository owner turns on Copilot code review for each repository that should use it.

1. Select **Project settings** > **Repos** > **Repositories**.
1. Select the repository you want to enable.
1. On the **Settings** tab, toggle **Enable Copilot code review for pull requests in this repository** to **On**.

   :::image type="content" source="media/copilot-code-reviews/repository-level-preview-feature.png" alt-text="Repository settings page with the Enable Copilot code review for pull requests toggle set to On.":::

To verify the feature is enabled, open any pull request in the repository. **GitHub Copilot** should now appear as an available reviewer in the **Reviewers** list.

## Use Copilot code review

With the feature enabled at all three scopes, you can ask Copilot to review a pull request. The following sections describe what to expect.

### Request a review

By default, **GitHub Copilot** reviews a pull request only when you ask for one:

1. Open a pull request.
1. In the **Reviewers** section, select **Request** next to **GitHub Copilot**.
1. Wait for the review to complete. The review might take a few moments, depending on the size of the repository and the number of changes in the pull request. When the review finishes, the status changes to **Review completed**.

If Copilot identifies potential issues, it adds comments and suggestions directly to the pull request for you to examine and address.

### Read Copilot's comments

- Copilot posts its feedback as a regular reviewer named **GitHub Copilot** on the pull request.
- Each comment appears on the line of code it applies to and, where possible, includes a suggested change that you can apply with one click.
- Copilot always leaves a **Comment** review. It never approves the pull request or requests changes, so its review doesn't satisfy required-reviewer policies and doesn't block merging.
- Copilot's comments behave like comments from a human reviewer. You can reply to them, react to them, resolve them, or hide them. Copilot doesn't read replies and doesn't follow up.

### Re-review after new commits

Copilot doesn't automatically re-review a pull request when you push new commits. To get a fresh review after a commit, select **Request** again next to **GitHub Copilot** in the **Reviewers** list.

<!-- Coming soon:
### Customize Copilot's reviews

 Azure Repos will honor `.github/copilot-instructions.md` and path-scoped `.github/instructions/**/*.instructions.md` files for tailoring Copilot's review behavior. 

For background on how custom instructions work in GitHub, see [Adding repository custom instructions for GitHub Copilot](https://docs.github.com/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot). 
-->

## Requirements and limits

The following requirements and limits apply during the preview and can change.

Copilot reviews a pull request only when it meets these requirements:

| Requirement | Value |
|--|--|
| Pull request status | **Active** |
| Pull request merge status | No merge conflicts (**Merge Succeeded**) |
| Repository size | 10 GB or less |
| Pull request changed files | 100 files or fewer |

These concurrency and rate limits also apply:

| Limit | Value |
|--|--|
| Duplicate review on the same pull request version | 1 completed review per merge commit |
| Concurrent reviews per pull request | 1 |
| Concurrent reviews per organization | 5 |
| Concurrent reviews per user | 2 |

## Billing

Each completed code review consumes tokens, including input tokens sent to the model, output tokens generated by the model, and cached tokens that reuse existing context. Tokens used for each review are converted into a standard billing unit called a *GitHub AI credit*, where 1 credit equals $0.01 USD.

Charges go to the Azure subscription linked to your Azure DevOps organization and appear as a separate meter in Azure Cost Management. The cost of each review varies based on factors like pull request size and the number of lines changed. To estimate expected costs in your environment, enable the feature for one or two repositories first and monitor daily usage.

To monitor your daily charges:

1. In the [Azure portal](https://portal.azure.com), go to your subscription.
1. Select **Cost Management** > **Cost analysis**.
1. Filter by product to view the organization's daily costs.

   :::image type="content" source="media/copilot-code-reviews/billing-cost-analysis.png" alt-text="Screenshot of Azure Cost Management Cost analysis filtered by product to show Copilot code review charges."::: 

### Set a budget alert

Create an Azure budget that notifies you when spending reaches a threshold you set. Budgets only notify you. They don't stop reviews or change any resources. You need **Owner**, **Contributor**, or **Cost Management Contributor** access on the subscription linked to your Azure DevOps organization.

1. In the [Azure portal](https://portal.azure.com), open the subscription linked to your Azure DevOps organization.
1. Select **Cost Management** > **Budgets**, and then select **Add**.
1. Under **Filters**, add a filter for **Product** and select **GitHub Copilot for AzDO**.

   :::image type="content" source="media/copilot-code-reviews/azure-portal-cost-management-budget-addition.png" alt-text="Screenshot of Azure Cost Management budget filters with Product set to GitHub Copilot for AzDO.":::

1. If the subscription is linked to multiple Azure DevOps organizations, add a filter for **Tag** and select the organization name tags you want the alert to target.
1. Enter a budget name, choose a reset period and expiration date, set the budget amount, and then select **Next**.
1. Add one or more alert thresholds as a percentage of the budget (for example, 75% and 90%), set **Type** to **Actual** or **Forecasted**, and enter the email addresses to notify.
1. Select **Create**.

   :::image type="content" source="media/copilot-code-reviews/create-azure-budget.png" alt-text="Screenshot of Azure Cost Management Budgets page with Add selected to create a new budget.":::

When spending reaches a threshold, Azure sends an email within an hour of the next evaluation. To review triggered alerts, select **Cost Management** > **Cost alerts**. To keep alert emails out of your junk folder, add `azure-noreply@microsoft.com` to your approved senders. For more information, see [Create and manage budgets](/azure/cost-management-billing/costs/tutorial-acm-create-budgets?tabs=psbudget).

## Turn off Copilot code review

To stop using Copilot code review, set the toggle to **Off** at the scope you want to disable:

- **For one user**: Turn off the **Preview features** toggle in your user settings.
- **For one repository**: Turn off the repository toggle in **Project settings** > **Repos** > **Repositories**.
- **For the entire organization**: Turn off the organization toggle in **Organization settings** > **Repos** > **Repositories**. This action disables the feature for all repositories.

## Share feedback

To report issues or share feedback about this preview, visit the [Azure DevOps Developer Community](https://developercommunity.visualstudio.com/AzureDevOps).

## Next step

> [!div class="nextstepaction"]
> [Review pull requests](review-pull-requests.md)

## Frequently asked questions (FAQs)

### Q: Where can I find the list prices that I'm charged for tokens?

A: See [Models and pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing#anthropic) in the GitHub Copilot documentation.

### Q: What factors influence the number of tokens consumed by a code review?

A: Token consumption depends on factors such as the size of the repository, the size of the change, and the complexity of the code being reviewed.

### Q: Do credits I purchase with Copilot subscriptions count toward code review usage in Azure DevOps? Can I use AI credits from a GitHub Copilot plan?

A: No, usage in Azure DevOps doesn't use AI credits from GitHub Copilot plans.

### Q: Is customer code or pull request review content used to train or improve foundation models?

A: No. Interaction data used for Copilot code review, including pull request diffs, prompts, responses, suggestions, and related review context, isn't used to train or improve foundation models.

### Q: What customer data is retained during or after a code review?

A: Data handling and retention follow GitHub Copilot policy documentation. Azure Repos doesn't publish a separate retention schedule for Copilot code review.

### Q: Are pull request diffs, prompts, generated suggestions, or telemetry stored beyond request processing? If retained, what are the retention periods?

A: For current storage and retention details, including any retention-related policy updates, see [GitHub Copilot trust and privacy documentation](https://copilot.github.trust.page/faq). Azure Repos doesn't currently publish a separate feature-specific retention period table for Copilot code review.

### Q: Where is customer content processed and stored for Copilot code review?

A: Copilot code review in Azure Repos is powered by GitHub Copilot. Data residency for GitHub Copilot doesn't align with Azure DevOps organization data residency boundaries for this preview feature. For example, if your Azure DevOps organization is hosted in the EU, Copilot code review processing might still occur in another geography, such as the United States.

### Q: Does GitHub Copilot Data Residency apply to Copilot code review for Azure Repos?

A: For this preview feature, don't assume Azure DevOps organization geography determines Copilot code review processing geography. Review [GitHub Copilot trust and privacy documentation](https://copilot.github.trust.page/faq) for current data residency scope and boundaries.

### Q: Does Copilot code review for Azure Repos follow the same data handling commitments as GitHub Copilot Business and GitHub Copilot Enterprise?

A: Yes, Copilot code review in Azure Repos follows GitHub Copilot data handling policies. For current commitments, see [GitHub Copilot trust and privacy documentation](https://copilot.github.trust.page/faq).

### Q: What customer-facing compliance documentation can I share with security and compliance teams?

A: Use these official references:

- [GitHub Copilot Trust Center FAQ](https://copilot.github.trust.page/faq)
- [GitHub General Privacy Statement](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement)
- [About GitHub Copilot code review](https://docs.github.com/copilot/using-github-copilot/code-review/using-copilot-code-review)

### Q: Are there preview-specific limitations or exceptions I should be aware of?

A: Yes. Copilot code review for Azure Repos is in limited public preview:

- Preview features can change or be removed without notice.
- Preview features have no Service Level Agreement (SLA) and limited support.
- Data residency for this feature doesn't align with Azure DevOps organization data residency boundaries.

## Related content

- [About GitHub Copilot code review](https://docs.github.com/copilot/using-github-copilot/code-review/using-copilot-code-review)
- [About pull requests](about-pull-requests.md)
- [Repository settings and policies](repository-settings.md)
