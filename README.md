# rm-resource-groups
Prevent your Azure bill from running wild by using GitHub Actions to remove Resource Groups conditionally. In this lesson I show how you can automate the removal of Azure resources on demand or by schedule. With GitHub Actions and an Azure account you can prevent services from running wild and causing unnecessary billing overcharges!

With automation in place, there is no need to remember what VM you left running!

First, you'll start by creating a GitHub Action that authenticates to Azure, then you will query locally your resource groups to identify which ones you can delete. Next, you will port the query over to the Action workflow. Finally, you will delete resources that match, making sure those are fully removed, preventing a large bill.


The video is available on [YouTube](https://www.youtube.com/watch?v=Nd4cwxROq30) as well as in the O'Reilly platform in the below link:
[![O'Reilly](https://learning.oreilly.com/covers/urn:orm:video:50141VIDEOPAIML/400w/)](https://learning.oreilly.com/videos/automated-azure-resource/50141VIDEOPAIML/ "Automated Azure Resource Cleanup")
> 🎥 Click the image above to access the full course on O'Reilly


## Check with consumption
Rough steps to double check with consumption over a certain dollar amount:

1. List all resource groups for the given subscription: `az group list`
2. Get consumption: with
```
curl -H "Authorization: Bearer $AZURE_TOKEN" -X GET https://management.azure.com/subscriptions/$AZURE_SUBSCRIPTION_ID_DEMOS/providers/Microsoft.Consumption/usageDetails?api-version=2021-10-01
```
3. From the consumption output, look at *an existing group* and then check dollar amount:
```python
In [15]: for item in usage:
    ...:     print(f"Resource Group: {item['properties']['resourceGroup']}")
    ...:     print(f"    Cost in USD: {item['properties']['costInUSD']}")
```
4. Nuke it with `az group delete -n $GROUP_NAME`

## Nuke it based on name
Query the list that contains a name and produce matches:

```
az group list --query "[?name.contains(@, 'demo')]" | jq '.[] | .name'
```

## 🥇 Learn Objectives

- 🚀  Create a GitHub Action workflow to query Resource Groups
- 🚀  Query Resource Groups that match locally
- 🚀  Use scripting to extract Resource Group names
- 🚀  Remove Resource Groups with the Azure CLI in GitHub Actions


## 🔗  R E S O U R C E S 

- [Sample GitHub repository](https://github.com/alfredodeza/rm-resource-groups)
- [Create an Azure Service Principal for GitHub Actions](https://learning.oreilly.com/videos/azure-in-github/50140VIDEOPAIML/)
- [Azure Service Principal documentation](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest&WT.mc_id=academic-0000-alfredodeza#az-ad-sp-create-for-rbac)
- [Install Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?WT.mc_id=academic-0000-alfredodeza)
- [Free Azure Certification for Students](https://docs.microsoft.com/learn/certifications/student-training-and-certification?WT.mc_id=academic-0000-alfredodeza)
- [Introduction to GitHub Actions](https://docs.microsoft.com/learn/modules/introduction-to-github-actions/?WT.mc_id=academic-0000-alfredodeza)
- [Run Python In GitHub Actions](https://learning.oreilly.com/videos/github-actions-and/50105VIDEOPAIML/)
