
# Sesión 07: Reto 02

## :dart: Objetivos

- Aprender a escribir pruebas unitarias para Triggers y Controladores
- Conocer las mejores practicas al escribir pruebas unitarias para Triggers y Controladores
- Aprender a usar la clase Limit para verificar si las pruebas sobrepasan los governor limit

## ⚙ Desarrollo
Escribir la clase de prueba para el siguiente Trigger

<strong>Trigger</strong>

```
trigger accountprefix on Account(before insert)
{
//for every new account
 for(Account a : Trigger.New)
 {
//append 'Mr.' with every account name 
  a.Name = 'Mr.' + a.Name;
 }
}
```

<strong>Clase de Prueba</strong>

```
@isTest //Annotation of test class
public class AccountInsert
{
//define testmethod 
 public static testmethod void testinsert()
 {
//Create new account instance and pass your name as string input 
  Account a = new Account(name = 'SalesforceKid');
//Append Mr. with the account name
  a.name = 'Mr.' + a.name;
  insert a;
  }
} 
```


