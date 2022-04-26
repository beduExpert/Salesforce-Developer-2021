
# Sesión 08: Creación de un servicio REST

## :dart: Objetivos

- Saber crear, publicar y probar un servicio REST.
- Configurar Salesforce para generar un servicio SOAP

## ⚙ Desarrollo

Es muy fácil exponer una clase como un servicio REST, todo lo que necesita hacer es lo siguiente:

1. Defina su clase como global.
2. Defina los métodos como estáticos globales.
3. Y agregue anotaciones a la clase y métodos.

Aquí tienes un código de ejemplo.

![image](https://user-images.githubusercontent.com/523243/145920937-57603f9a-403c-41dd-8856-d23244835b00.png)

```
For class : @RestResource(urlMapping='/Account/*')
For methods :
Annotation Action Details
@HttpGet ===>>> Reads or retrieves records.
@HttpPost ===>>> Creates records.
@HttpDelete ===>>> Deletes records.
@HttpPut ===>>> Typically used to update existing records or create records.
@HttpPatch ===>>> Typically used to update fields in existing records.
REST Endpoint for Above class
```

![image](https://user-images.githubusercontent.com/523243/145921018-e0a385e3-8645-43a0-a1ae-8399c64ed113.png)

Ahora, creemos la clase.

![image](https://user-images.githubusercontent.com/523243/145921069-0536ac98-dbd9-4929-a92b-c099522e202d.png)

Cree un método para obtener cuentas de Salesforce.

![image](https://user-images.githubusercontent.com/523243/145921126-ac839e45-60bd-4b34-a90c-537f1d1d5d0d.png)

Cree un método para crear una cuenta en Salesforce.

![image](https://user-images.githubusercontent.com/523243/145921186-a3f3516c-4f29-4491-8eba-54a87cb2b974.png)

Clase final con ambos métodos.

![image](https://user-images.githubusercontent.com/523243/145921236-8bcccbae-5687-429f-b177-343d655d8072.png)

Hemos terminado con el código Apex. Ahora los haremos accesibles con autenticación usando el sitio de Salesforce.
Registremos el sitio en Salesforce Org.<br/>

Setup > Site> Register

Primero, verifique la disponibilidad y luego regístrese para la misma.

![image](https://user-images.githubusercontent.com/523243/145921446-750619c0-91c4-43b8-95bc-162d89b2829d.png)

Ahora, cree un sitio nuevo.

![image](https://user-images.githubusercontent.com/523243/145921504-2aedf69d-a04c-4e9a-ab0f-05825e532fc3.png)

Edite el sitio de acceso público.

![image](https://user-images.githubusercontent.com/523243/145921580-ce7b3f13-4347-4c52-99c4-3a24c0733318.png)

Habilite el acceso a clases de Apex y de acceso a clases recién creadas.

![image](https://user-images.githubusercontent.com/523243/145921655-d72ad8e5-d70d-4e40-b01c-d242b97026bc.png)

Todo está listo ahora.

El endpoint final sera: heysite-developer-edition.ap16.force.com/heySalesforce/services/apexrest/Account/
Site Domain URL: http://heysite-developer-edition.ap16.force.com/
Site Name: heySalesforce
Apex Service: services/apexrest/Account/


