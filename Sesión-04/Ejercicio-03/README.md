
# Sesión 04: Contruir un marco para un Trigger

## :dart: Objetivos

- Escribir Trigger eficientes incluso en operaciones masivas
- Aprender a evitar el diseño de Triggers que superen los governor limits

## ⚙ Desarrollo

Cada Trigger debe implementarse en una custom SettING que permita que el Trigger esté activo / inactivo desde la interfaz de usuario.

<Strong>Custom Settings: (Trigger Setting)</Strong>

![image](https://user-images.githubusercontent.com/523243/145913659-73ad4918-595a-4ae7-ba9a-fbebedd4f18c.png)

<Strong>Account Object Trigger: (AccountTrigger)</Strong>

```
trigger AccountTrigger on Account(before insert, after insert, before update, after update, before delete, after delete, after unDelete) {
    TriggerDispatcher.run(new AccountTriggerHandler(), 'AccountTrigger');
}
```

<Strong>Account Object Trigger Handler: (AccountTriggerHandler)</Strong>
  
```
public class AccountTriggerHandler implements ITriggerHandler{
     
    //Use this variable to disable this trigger from transaction
    public static Boolean TriggerDisabled = false;
     
    //check if the trigger is disabled from transaction
    public Boolean isDisabled(){
        return TriggerDisabled;
    }
     
    public void beforeInsert(List<sObject> newList) {
         
    }
     
    public void afterInsert(List<sObject> newList , Map<Id, sObject> newMap) {
         
    }
     
    public void beforeUpdate(List<sObject> newList, Map<Id, sObject> newMap, List<sObject> oldList, Map<Id, sObject> oldMap) {
         
    }
     
    public void afterUpdate(List<sObject> newList, Map<Id, sObject> newMap,  List<sObject> oldList, Map<Id, sObject> oldMap) {
         
    }
     
    public void beforeDelete(List<sObject> oldList , Map<Id, sObject> oldMap) {
         
    }
     
    public void afterDelete(List<sObject> oldList , Map<Id, sObject> oldMap) {
         
    }
     
    public void afterUnDelete(List<sObject> newList, Map<Id, sObject> newMap) {
         
    }
}
```

<Strong>Trigger Interface: (ITriggerHandler)</Strong>

```
public interface ITriggerHandler{
     
    void beforeInsert(List<sObject> newList);
     
    void afterInsert(List<sObject> newList, Map<Id, sObject> newMap);
     
    void beforeUpdate(List<sObject> newList, Map<Id, sObject> newMap,  List<sObject> oldList, Map<Id, sObject> oldMap);
     
    void afterUpdate(List<sObject> newList, Map<Id, sObject> newMap,  List<sObject> oldList, Map<Id, sObject> oldMap);
     
    void beforeDelete(List<sObject> oldList , Map<Id, sObject> oldMap);
     
    void afterDelete(List<sObject> oldList , Map<Id, sObject> oldMap);
     
    void afterUnDelete(List<sObject> newList, Map<Id, sObject> newMap);
     
    Boolean isDisabled();
}
```

<Strong>Trigger Dispatcher Class:(TriggerDispatcher)</Strong>

```
public class TriggerDispatcher {
     
    public static void run(ITriggerHandler handler, string triggerName){
         
        //Check if the trigger is disabled
        if (handler.IsDisabled()){
            return;
        }
         
        //Get the trigger active information from custom settings by trigger name
        Boolean isActive = TriggerSetting__c.getValues(triggerName).isActive__c;
         
        if(isActive){
            //Check trigger context from trigger operation type
            switch on Trigger.operationType {
                 
                when BEFORE_INSERT {
                    //Invoke before insert trigger handler
                    handler.beforeInsert(trigger.new);
                }
                when AFTER_INSERT {
                    //Invoke after insert trigger handler
                    handler.afterInsert(trigger.new, trigger.newMap);
                }
                when BEFORE_UPDATE {
                    //Invoke before update trigger handler
                    handler.beforeUpdate(trigger.new, trigger.newMap, trigger.old, trigger.oldMap);
                }
                when AFTER_UPDATE {
                    //Invoke after update trigger handler
                    handler.afterUpdate(trigger.new, trigger.newMap, trigger.old, trigger.oldMap);
                }
                when BEFORE_DELETE {
                    //Invoke before delete trigger handler
                    handler.beforeDelete(trigger.old, trigger.oldMap);
                }
                when AFTER_DELETE {
                    //Invoke after delete trigger handler
                    handler.afterDelete(trigger.old, trigger.oldMap);
                }
                when AFTER_UNDELETE {
                    //Invoke after undelete trigger handler
                    handler.afterUnDelete(trigger.new, trigger.newMap);
                }
            }
        }
    }
}
```





