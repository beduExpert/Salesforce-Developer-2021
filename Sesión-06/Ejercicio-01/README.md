
# Sesión 06: Creando una pagina en Visualforce

## :dart: Objetivos
- Conocer el concepto de Modelo, Vista, Controlador
- Crear tu primera pagina en Visualforce
- Crear una cuenta asociada con un Prospecto desde una pagina Custom

## ⚙ Desarrollo

1) Cree un nuevo controlador asociado con el objeto Oportunidad (Lead)
2) Cree una nueva página Visualforce asociada con el controlador creado en el paso 1 y el objeto "Lead".
3) En el Layout de la página creada, debe poder capturar los siguientes campos:

- Prioridad
- Número de la Cuenta Asociada

4) Debe existir un botón "Guardar" en la página.
5) En el código del controlador, coloque el código necesario dentro del "Constructor" para inicializar las variables.
6) Coloque el código necesario para llenar las opciones de ComboBox “Prioridad” dentro del controlador.
7) Dentro del evento Guardar del controlador, validar que no exista la cuenta ingresada de acuerdo a lo que el usuario capture en el campo "Número de Cuenta Asociada".
8) Si lo hay, envíe un mensaje de error y no cree la cuenta. De lo contrario, guarde la nueva cuenta, recuperando los campos de Lead:

- Descripción
- Nombre
- Sitio web
- Clasificación

Estos campos deben guardarse en la nueva cuenta, junto con los campos de captura:

Prioridad
Número de la Cuenta Asociada

Si no hay ningún error, debe navegar a la nueva cuenta creada. De lo contrario, se debe enviar un mensaje de error.

9) En el formato de objeto Lead, cree una nueva acción asociada a la página, con la leyenda "Cuenta asociada".
10) Modifique el diseño de la página principal para agregar la nueva acción creada en el paso 8.
11) Cree un nuevo Lead y pruebe la funcionalidad generada en el ejercicio.


<strong> Controlador </strong>

```
  
  public with sharing class myFirstPage_cls {
   
    //Parent controller
    private final SObject parent; 

    //Valores seleccionados en los picklist
    public String selPrioridad {set;get;}
    public list<selectoption> getCmbPrioridad {get;set;}

	 // Valores adicionales de la página
   public String sMsg{get;set;}
   public boolean INPUT{get;set;}
   public String noCta{get;set;}

    // Variable para instancia de nueva cuenta
   public String sID;
   public Lead objLead = new Lead();
   public Account chkAcctDoc = new Account();
    
        // Método para guardar
    public pageReference save(){   
		//Variables auxiliares
		String sAccID;
        String sNoDocProspect;
        Boolean bExistDoc;
       
     // Recupera datos del lead actual
       objLead = [select Description, Name, Website, Rating
               from Lead where id = :sID];

       // Válida que no exista el mismo número
        try {
       chkAcctDoc = [select id, AccountNumber from Account 
                     where AccountNumber = :noCta];
            
            bExistDoc = true;
        } catch (exception e) {
            bExistDoc = false;
        }
        
        if (bExistDoc == false) {
         // Agrega nuevo registro, conforme mapeo
           Account AccountNew = new Account();
           AccountNew.Description = objLead.Description;
           AccountNew.Name = objLead.Name;
           AccountNew.Website = objLead.Website;
           AccountNew.Rating = objLead.Rating;
           AccountNew.AccountNumber = noCta;
           AccountNew.CustomerPriority__c = selPrioridad;
           
            // Inserta regristro
           try{
               insert AccountNew;
               sAccID = AccountNew.id;
               
           // Actualiza Lead
               try {
                   //navigateToSObject(AccountNew.id);
                    PageReference ref= new PageReference('/'+AccountNew.id);
                    ref.setredirect(true);
                    return ref;
               } catch (exception e){
               sAccID = '';
               sMsg = e.getMessage();
               }
            }
            catch(exception e){
               sAccID = '';
               sMsg = e.getMessage();
            }
        } else {
            sAccID = '';
            sMsg = 'El número de documento: ' + noCta + ' ya existe en la cuenta ID: ' + chkAcctDoc.id + '.';
        }

                   // Ajusta mensaje de salida
        if (sAccID <> '') {
            sMsg = 'Se ha creado la cuenta: ' + sAccID;
        }
        
        //Envío de mensaje
        	INPUT = false;
            
		// Regresa a la página actual
        return null;
    }

    //constructor, limpia, los valores de selección
    public myFirstPage_cls(ApexPages.StandardController controller) {
        // Llena las variables, en iniciales 
        parent = controller.getRecord();
        
	     sID = parent.id;
		 selPrioridad = '';
         INPUT = true;
        
        		// Llena la lista de prioridad
        List<SelectOption> PrioridadList = new List<SelectOption>();
        PrioridadList.add(new SelectOption('High ' , 'High'));
        PrioridadList.add(new SelectOption('Low' , 'Low'));
        PrioridadList.add(new SelectOption('Medium' , 'Medium'));
        
        getCmbPrioridad = PrioridadList;

    }
}

```

<strong> Visualforce page </strong>


 
```
  <apex:page standardcontroller="Lead" extensions="myFirstPage_cls" lightningStylesheets="true" docType="html-5.0" showHeader="true" showQuickActionVfHeader="false">
  <apex:sectionHeader title="Conversión de Prospecto a Cuenta"/>
    <apex:form >
    <apex:pageblock >    
        <apex:pageblockSection rendered="{!INPUT==true}">
            <apex:outputPanel >
            <table>
             <tr>
             <td><apex:outputlabel value="Prioridad:" /></td>
               <td>
                <apex:selectList value="{!selPrioridad}" size="1" required="true">
                <apex:selectOptions value="{!getCmbPrioridad}" ></apex:selectOptions>
                </apex:selectList>
               </td>
             </tr>
             <tr>
             <td><apex:outputlabel value="Número de la cuenta:" /></td>  
             <td><apex:input type="auto" value="{!noCta}" required="true" /></td>
             </tr>
            </table>
            <center>
            <apex:commandbutton value="Guardar" action="{!save}" />
            </center>
            </apex:outputPanel>    
        </apex:pageblockSection> 
        <apex:pageblockSection rendered="{!INPUT==false}">
            <apex:outputPanel >
             <center>
             <apex:outputText >{!sMsg}</apex:outputText>
             </center>
            </apex:outputPanel>    
        </apex:pageblockSection> 
        
    </apex:pageblock>   
  </apex:form>
</apex:page>

```
