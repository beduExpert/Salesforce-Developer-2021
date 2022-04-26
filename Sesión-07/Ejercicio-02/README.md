
# Sesión 07: Crea una clase de pruebas para Triggers

## :dart: Objetivos

- Aprender a escribir pruebas unitarias para Triggers y Controladores
- Conocer las mejores practicas al escribir pruebas unitarias para Triggers y Controladores
- Aprender a usar la clase Limit para verificar si las pruebas sobrepasan los governor limit

## ⚙ Desarrollo

Escribir el Trigger y su clase de prueba

<Strong>Trigger</Strong>

```
trigger accounInsert on Account(before insert)
{
//Check for all the new account record in for loop
 for(Account a : Trigger.New)
 {
//Query
  List<Account> mynew = [SELECT Id, Name FROM Account WHERE                                                      Name= :a.Name];
  if(mynew.size() > 0)
  {
//Add Error message if there is record after querying 
   a.Name.addError('Account with name is existing'); 
  }
 }
}
```

<strong>Clase de Prueba</strong>

 ```
  @isTest //Annotation so that salesforce can undertand this is test class
public class AccountInsert
{
//Defind testmethod keyword while defining method 
 public static testMethod void testInsert()
 {
//Defind variables
  String addError;
  String myname = 'SalesforceKid';
//Create an instance of an object on which you want to check this trigger is working or not. 
  Account a2 = new Account(name = myname);
//Query 
  List<Account> x =[SELECT Id, Name FROM Account WHERE Name=                                         :myname];
 if(x.size() < 1)
  {
//Check if list is empty  
   System.assertEquals(0, x.size());
//Insert the record
   Insert a2;
  }
else
 {
//Otherwise show error if there is something in the list with same name
  addError ='Existing';
  }
//Check whether the error you are getting is similar to the one you added
System.assertEquals('Existing', addError);
 }
}
```



