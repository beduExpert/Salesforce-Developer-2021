
# Sesión 07: Tu primera Clase de Prueba

## :dart: Objetivos

- Comprender el marco de pruebas unitarias en apex
- Crear datos de pruebas para clases Apex
- Describir las diferentes herramientas que pueden ser usadas en clases de prueba

## ⚙ Desarrollo

Construye una clase de pruebas para el metodo <strong>testDeleteAccountSalary</strong>

```
@isTest static void testDeleteAccountSalary(){
        Test.startTest();
            delete [Select Id from Account_Salary__c where Name='as2'][0];
        Test.stopTest();
         
        Account acc = [Select Id, Max_Salary__c, Total_Salary__c from Account][0];
        System.assertEquals(acc.Max_Salary__c, 500);
        System.assertEquals(acc.Total_Salary__c, 500);
    }
}
```



