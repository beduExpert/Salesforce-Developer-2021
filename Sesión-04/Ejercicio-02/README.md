
# Sesión 04: Construir un Trigger

## :dart: Objetivos

- Aprender a escribir un Trigger para un objeto en Salesforce
- Hacer una validacion en el objeto cuenta
- Verificar que la validacion se ejecuta antes de la creacion del objeto cuenta

## ⚙ Desarrollo

En VisualStudio crea un nuevo Trigger usando la opcion sobre la carpeta Triggers

![image](https://user-images.githubusercontent.com/523243/145733960-aa939195-f6a0-4bed-960d-8620e9221ef5.png)

Asigna un nombre al Trigger

![image](https://user-images.githubusercontent.com/523243/145734005-402a3282-b03f-4b1b-96d2-96282f971bbe.png)

Modifica el Trigger para que tenga la siguiente sintaxis.

![image](https://user-images.githubusercontent.com/523243/145734088-b0494e9b-5777-4ae1-a84d-4ab410fa9f25.png)

Crea una clase Helper, revisa en el repositorio la sintaxis de la clase

![image](https://user-images.githubusercontent.com/523243/145734136-68071818-4505-4a43-a701-dbe52cdb72c7.png)

Implementa la clase Helper en el Trigger.

![image](https://user-images.githubusercontent.com/523243/145734153-d2954523-1b17-4526-815f-0cfbeb8d49ee.png)

<Strong>Trigger</Strong>

```
trigger AccountSalaryTrigger on Account_Salary__c (after insert, after update, after delete, after undelete) {
    if (Trigger.isUpdate){
        AccountSalaryHelper.updateAccount(Trigger.new, Trigger.oldMap);
    } else if (Trigger.isDelete){
       AccountSalaryHelper.updateAccount(Trigger.old, null);
    } else {
       AccountSalaryHelper.updateAccount(Trigger.new, null);
    }
}
```

<strong>Clase Helper</strong>

```
public with sharing class AccountSalaryHelper {
 
    public static void updateAccount(List<Account_Salary__c> newAccountSalaries, Map<Id, Account_Salary__c> oldMap){
        Set<Id> accountIds = new Set<Id>();
        for (Account_Salary__c newAccountSalary : newAccountSalaries){
            if (oldMap!=null){
                Account_Salary__c oldAccountSalary = oldMap.get(newAccountSalary.Id);
                if (oldAccountSalary.Salary__c != newAccountSalary.Salary__c){
                    accountIds.add(newAccountSalary.Account__c);
                }
            } else {
                accountIds.add(newAccountSalary.Account__c);
            }
        }
         
        if (!accountIds.isEmpty()){
            List<AggregateResult> aggResults = [Select Account__c accId, sum(Salary__c) sumSalary, max(Salary__c) maxSalary from Account_Salary__c where Account__c IN :accountIds Group By Account__c];
             
            List<Account> accountsToUpdate = new List<Account>();
            for (AggregateResult aggResult : aggResults){
                Id accountId = (Id)aggResult.get('accId');
                if (accountId!=null){
                    Account updateAccount = new Account();
                    updateAccount.Id =accountId;
                    updateAccount.Total_Salary__c=(Decimal)aggResult.get('sumSalary');
                    updateAccount.Max_Salary__c = (Decimal)aggResult.get('maxSalary');
                    accountsToUpdate.add(updateAccount);
                }
            }
             
            if (!accountsToUpdate.isEmpty()){
                SavePoint sp = Database.setSavePoint();
                try{
                    update accountsToUpdate;
                } catch(DMLException ex){
                    Database.rollback(sp);
                }
            }
        }
    }
}
```





