# rm-resource-groups
Use GitHub Actions to remove Resource Groups conditionally 

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
