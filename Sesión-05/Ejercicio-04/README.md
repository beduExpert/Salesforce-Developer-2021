
# Sesión 05: Procesos encolados con Apex

## :dart: Objetivos

- Aprender a construir un proceso encolado en Apex
- Conocer cómo implementar la interfaz Queueable
- Identificar las diferencias entre Queueable y Feature

## ⚙ Desarrollo

Construye la siguiente clase:

```
public class AccountQueueableExample implements Queueable {
    public List<Account> accList ; 
    public AccountQueueableExample(List<Account> accList){
        this.accList = accList ;  
    }
    public void execute(QueueableContext context) {
        for(Account acc :accList){
            // Update the Account Name 
            acc.Name = acc.Name + 'Bedu';
        }
        update accList;
    }
}
```
  
Y su clase de prueba

```
@isTest
public class AccountQueueableExampleTest {
    @testSetup
    static void setup() {
        List<Account> accounts = new List<Account>();
        // add 100 accounts
        for (Integer i = 0; i < 100; i++) {
            accounts.add(new Account(
                name='Test Account'+i
            ));
        }
        insert accounts;
    }
     
    static testmethod void testQueueable() {
        // query for test data to pass to queueable class
        List<Account> accounts = [select id, name from account where name like 'Test Account%'];
        // Create our Queueable instance
        AccountQueueableExample accQObj = new AccountQueueableExample(accounts);
        // startTest/stopTest block to force async processes to run
        Test.startTest();        
        System.enqueueJob(accQObj);
        Test.stopTest();        
        // Validate the job ran
        System.assertEquals(100, [select count() from account where Name like = '%Bedu%']);
    }
     
}
```

Prueba el job ejecutando el siguiente codigo
```
List<Account> accList = [Select Id , Name from Account WHERE [Condition] ];
ID jobID = System.enqueueJob(new AccountQueueableExample(accList));
System.debug('jobID'+jobID);
```







