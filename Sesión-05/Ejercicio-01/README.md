
# Sesión 05: Procesamiento Batch

## :dart: Objetivos


- Entender los conceptos basicos del procesamiento batch
- Aprender la correcta sintaxis para implementar procesos en batch
- Construir un job con clases batch

## ⚙ Desarrollo

Crear una clase Apex con la siguiente sintaxis

![image](https://user-images.githubusercontent.com/523243/145734572-9c8a6bc7-6b65-4a13-aac6-e02f94fe9816.png)

Ajustar la sintaxis de la clase para que sea una clase batch apex

![image](https://user-images.githubusercontent.com/523243/145734597-45e3166f-83aa-45a3-a406-70ad4610f621.png)

Desplegar la clase en la scratch org e invocarla por la developer console usando la siguiente sintaxis

MyBatchClass myBatchObject = new MyBatchClass(); 
Id batchId = Database.executeBatch(myBatchObject);

![image](https://user-images.githubusercontent.com/523243/145734637-aece733c-39b9-416c-beb7-1d5e1b880154.png)

Cree una nueva clase Apex

![image](https://user-images.githubusercontent.com/523243/145734659-cf82b8e4-3a9e-41f9-938d-81afff9159b4.png)

Ajuste la nueva clase para que implemente la interface Schedulable e invoque la clase Batch Apex

![image](https://user-images.githubusercontent.com/523243/145734673-ec29bcb3-2d5a-4048-9194-f05e8ff82e1a.png)




