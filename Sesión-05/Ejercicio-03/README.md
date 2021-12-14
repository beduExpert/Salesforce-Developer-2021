
# Sesión 05: Process Builder con Apex

## :dart: Objetivos

- Aprender a integrar clases Apex con process builder
- Aprender a integrar clases Apex con flows
- Conocer en qué casos usar la integración de clases Apex con process builder o flows
-
## ⚙ Desarrollo

<strong>Ejemplo de caso de uso o escenario</strong>

Suponga un caso de negocio en el cual un usuario tiene que crear una cantidad de contactos para una cuenta. Los usuarios normalmente tienen que hacer clic en muchos campos para poder crear la cantidad de contactos requeridos. Una forma de automatización que espera el cliente es agregar un campo "Número de contactos" al crear una cuenta. Con base en este número, el cliente espera crear "N" registros de contactos básicos automáticamente. Los usuarios luego completarán o actualizarán el campo correspondiente en los contactos. Se espera que esto ahorre una cantidad sustancial de tiempo y también mejore la adopción por parte de los usuarios.

Este ejemplo a continuación explica cómo crear la clase Apex y el Process builder para "Insertar 'n' número de contactos según el campo 'Número de contactos' mientras se crea la cuenta". Con estos pasos, puede comprender cómo llamar a apex desde Process builder.

1) Crea un campo custom "Numero de contactos" en el objeto Cuenta (Account) (Tipo de dato: Number).
2) Crea una clase apex usando el siguiente codigo:

```
/**
 Class Name: createchildRecords
 Purpose: Create N number of contacts based on the field ‘Number of contacts’ while Account is created.  
**/
 
public class CreateChildRecords  {
@InvocableMethod (label=’Create Contacts’) 
public Static void createContact(List<Id> accIds){ 
List<Contact> conListToInsert = new List<Contact>(); //list to collect and insert the contacts 
//Query the accounts 
List<Account> accList = [Select Id, Name, Number_of_Contacts__c from Account where Id =:accIds];
//loop through the accounts and create contacts
for(Account acc : accList)
{
if(acc.Number_of_Contacts__c != null)
{
for(integer i=1;i<=acc.Number_of_Contacts__c;i++)
{
Contact con = new Contact();
con.LastName = ‘Invocable’ + acc.Name +’ ‘+ i;
con.AccountId = acc.Id;
conListToInsert.add(con);
 }  }  }
if(!conListToInsert.isEmpty()) {
insert conListToInsert; }   } }
 ```

3) Crea el flujo usando process builder

Process builder es una herramienta poderosa que puede utilizar para automatizar los procesos comerciales. Tiene una interfaz simple que le permite apuntar y hacer click para seleccionar objetos y campos mientras configura acciones inmediatas y basadas en el tiempo.

  a) Desde Configuración, ingrese a Process Builder en el cuadro de búsqueda rápida.<br/>
  b) Haga clic en Process Builder y luego haga clic en Nuevo.<br/>
  c) Asigne un nombre el proceso (por ejemplo, crear contactos).<br/> 
  d) El nombre de la API se actualiza automáticamente. Seleccione "Registrar cambios" y haga clic en "Guardar".<br/>
  e) Haga clic en "+" y agregue un objeto para asociar su proceso con un objeto. Para ello, puede elegir el objeto Cuenta y vincular el proceso al objeto Cuenta.<br/>
  f) Seleccione "solo cuando se crea un registro" y haga clic en "Guardar".<br/> 
  g)Ingrese la condición para verificar si el "Número de contactos" es mayor que 0. Si el usuario ingresa el número de contactos "0", el código no se ejecutará.<br/>
  h)Ingrese el nombre del criterio "Número de contactos"
       Criterios para ejecutar acciones: seleccione "Se cumplen las condiciones"
       Establecer condiciones: Campo - Ingrese “Account.Number_of_Contacts__c”; Operador: seleccione "Mayor que"; Tipo: seleccione "Número"; Valor: ingrese "0"
                                                                                                                                                                                    ![image](https://user-images.githubusercontent.com/523243/145915805-12ac3eaa-f026-4639-84b7-5584f42fdf19.png)


<strong>Definir criterios basados en la condición</strong>

4) A continuación, agregue acciones para ejecutar cuando se cumplan los criterios

a) En acciones inmediatas, haga clic en Agregar acción.<br/>
b) Seleccione la clase Apex en el tipo de acción.<br/>
c) Ingrese el nombre en el nombre de la acción.<br/>
d) Las clases con el método invocable están disponibles en el campo de clase apex. Seleccione la clase deseada.<br/>
e) Posteriormente, también puede asignar parámetros al método. Para esto, haga clic en la fila para agregar en la sección de configuración de apex.<br/>
f) Seleccione el campo del método de la clase apex.<br/>
g) Establecer tipo de valor. (Puede seleccionar entre fórmula, constante global, referencia de campo e id).<br/>
h) Ingrese el valor y haga clic en guardar.<br/>

![image](https://user-images.githubusercontent.com/523243/145916374-7e75734f-1c82-4992-9a2a-145a0febc039.png)


Agregue la acción con el tipo de acción apex

5. Pruebe los resultados de llamar a apex desde Process Builder.<br/>
a) Navegue hasta el objeto Cuenta, cree una nueva cuenta e ingrese 4 en el "Número de contactos".<br/>
b) Guarde la cuenta.<br/>
c) Consulta el objeto de contactos.<br/>

<strong>Se crearán cuatro contactos para la cuenta con detalles básicos.</strong>

![image](https://user-images.githubusercontent.com/523243/145916560-cad8712d-107a-4f26-9e71-ff3624933c1b.png)



