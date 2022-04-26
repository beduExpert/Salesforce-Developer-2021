
# Sesión 08: Consumo de Servicio SOAP

## :dart: Objetivos

- Preparar una clase Apex para el consumo de un Servicio Web Externo a Salesforce
- Conocer los protocolos SOAP, REST y sus diferencias.

## ⚙ Desarrollo

Para este ejercicio se utilizará el servicio web descrito en el siguiente manual: http://www.dneonline.com/calculator.asmx y el WSDL en la siguiente URL: http://www.dneonline.com/calculator.asmx?WSDL.

Sin embargo, el WSDL recibió algunos detalles para ser interpretados por la herramienta WSDL2Apex. El WSDL final es el siguiente:

```
<?xml version="1.0" encoding="utf-8"?>


<wsdl:definitions xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/" xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/" xmlns:mime="http://schemas.xmlsoap.org/wsdl/mime/" xmlns:tns="http://tempuri.org/" xmlns:s="http://www.w3.org/2001/XMLSchema" xmlns:http="http://schemas.xmlsoap.org/wsdl/http/" targetNamespace="http://tempuri.org/" xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/">


  <wsdl:types>


    <s:schema elementFormDefault="qualified" targetNamespace="http://tempuri.org/">


      <s:element name="Add">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="intA" type="s:int" />


            <s:element minOccurs="1" maxOccurs="1" name="intB" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="AddResponse">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="AddResult" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="Subtract">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="intA" type="s:int" />


            <s:element minOccurs="1" maxOccurs="1" name="intB" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="SubtractResponse">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="SubtractResult" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="Multiply">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="intA" type="s:int" />


            <s:element minOccurs="1" maxOccurs="1" name="intB" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="MultiplyResponse">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="MultiplyResult" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="Divide">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="intA" type="s:int" />


            <s:element minOccurs="1" maxOccurs="1" name="intB" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


      <s:element name="DivideResponse">


        <s:complexType>


          <s:sequence>


            <s:element minOccurs="1" maxOccurs="1" name="DivideResult" type="s:int" />


          </s:sequence>


        </s:complexType>


      </s:element>


    </s:schema>


  </wsdl:types>


  <wsdl:message name="AddSoapIn">


    <wsdl:part name="parameters" element="tns:Add" />


  </wsdl:message>


  <wsdl:message name="AddSoapOut">


    <wsdl:part name="parameters" element="tns:AddResponse" />


  </wsdl:message>


  <wsdl:message name="SubtractSoapIn">


    <wsdl:part name="parameters" element="tns:Subtract" />


  </wsdl:message>


  <wsdl:message name="SubtractSoapOut">


    <wsdl:part name="parameters" element="tns:SubtractResponse" />


  </wsdl:message>


  <wsdl:message name="MultiplySoapIn">


    <wsdl:part name="parameters" element="tns:Multiply" />


  </wsdl:message>


  <wsdl:message name="MultiplySoapOut">


    <wsdl:part name="parameters" element="tns:MultiplyResponse" />


  </wsdl:message>


  <wsdl:message name="DivideSoapIn">


    <wsdl:part name="parameters" element="tns:Divide" />


  </wsdl:message>


  <wsdl:message name="DivideSoapOut">


    <wsdl:part name="parameters" element="tns:DivideResponse" />


  </wsdl:message>


  <wsdl:portType name="CalculatorSoap">


    <wsdl:operation name="Add">


      <wsdl:documentation xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/">Adds two integers. This is a test WebService. ©DNE Online</wsdl:documentation>


      <wsdl:input message="tns:AddSoapIn" />


      <wsdl:output message="tns:AddSoapOut" />


    </wsdl:operation>


    <wsdl:operation name="Subtract">


      <wsdl:input message="tns:SubtractSoapIn" />


      <wsdl:output message="tns:SubtractSoapOut" />


    </wsdl:operation>


    <wsdl:operation name="Multiply">


      <wsdl:input message="tns:MultiplySoapIn" />


      <wsdl:output message="tns:MultiplySoapOut" />


    </wsdl:operation>


    <wsdl:operation name="Divide">


      <wsdl:input message="tns:DivideSoapIn" />


      <wsdl:output message="tns:DivideSoapOut" />


    </wsdl:operation>


  </wsdl:portType>


  <wsdl:binding name="CalculatorSoap" type="tns:CalculatorSoap">


    <soap:binding transport="http://schemas.xmlsoap.org/soap/http" />


    <wsdl:operation name="Add">


      <soap:operation soapAction="http://tempuri.org/Add" style="document" />


      <wsdl:input>


        <soap:body use="literal" />


      </wsdl:input>


      <wsdl:output>


        <soap:body use="literal" />


      </wsdl:output>


    </wsdl:operation>


    <wsdl:operation name="Subtract">


      <soap:operation soapAction="http://tempuri.org/Subtract" style="document" />


      <wsdl:input>


        <soap:body use="literal" />


      </wsdl:input>


      <wsdl:output>


        <soap:body use="literal" />


      </wsdl:output>


    </wsdl:operation>


    <wsdl:operation name="Multiply">


      <soap:operation soapAction="http://tempuri.org/Multiply" style="document" />


      <wsdl:input>


        <soap:body use="literal" />


      </wsdl:input>


      <wsdl:output>


        <soap:body use="literal" />


      </wsdl:output>


    </wsdl:operation>


    <wsdl:operation name="Divide">


      <soap:operation soapAction="http://tempuri.org/Divide" style="document" />


      <wsdl:input>


        <soap:body use="literal" />


      </wsdl:input>


      <wsdl:output>


        <soap:body use="literal" />


      </wsdl:output>


    </wsdl:operation>


  </wsdl:binding>


  <wsdl:service name="Calculator">


    <wsdl:port name="CalculatorSoap" binding="tns:CalculatorSoap">


      <soap:address location="http://www.dneonline.com/calculator.asmx" />


    </wsdl:port>


  </wsdl:service>


</wsdl:definitions>

```

<strong>Importación de WSDL a Salesforce mediante WSDL2Apex</strong>

Para importar WSDL a Salesforce, vamos a iniciar sesión en nuestra organización de Salesforce y seguir los pasos a continuación: -
 
1. Vaya a Configuración, busque Clases de Apex y abra la página Clases de Apex.

![image](https://user-images.githubusercontent.com/523243/145919083-7d41e2c6-5f67-4b9e-a5c7-4e7284a1cc3f.png)

2. Verá que en la página de Clases de Apex, hay un botón llamado Generar desde WSDL que se usa para importar WSDL a Salesforce. Haga clic en ese botón y verá la siguiente pantalla.

![image](https://user-images.githubusercontent.com/523243/145919136-2bdef3a7-e881-40c0-a7e3-6634ba6efef3.png)

3. Haga clic en el botón Elegir archivo y seleccione su archivo WSDL, que no es más que el mismo archivo XML que escribimos al comienzo del ejercicio. WSDL significa principalmente lenguaje de descripción de servicios web. El archivo WSDL consta de la descripción detallada del servicio web SOAP, que consta de la URL del servicio web y los formatos de solicitud / respuesta para las diferentes operaciones que se pueden realizar utilizando la API de SOAP. Aquí, he guardado el XML anterior en un archivo conocido como calculator.xml y lo he elegido como se muestra a continuación.

![image](https://user-images.githubusercontent.com/523243/145919265-8862be77-16b1-4e15-a6d6-64fc91acd392.png)

4. Haga clic en el botón Parse WSDL. Verá la siguiente pantalla.

![image](https://user-images.githubusercontent.com/523243/145919352-68e578ac-7030-45f7-8e62-faa820e19fb4.png)

5. Puede cambiar el nombre de clase de Apex a lo que desee, lo estoy actualizando a CalculatorService como se muestra a continuación.

![image](https://user-images.githubusercontent.com/523243/145919412-8e8aaa91-fbec-43f7-b91a-ca4f1ef3d6d6.png)

6. Haga clic en el botón Generar código Apex. Creará automáticamente las clases de Apex a partir del WSDL importado y verá la siguiente pantalla que muestra los nombres de dos clases de Apex recién creadas en su organización, es decir, CalculatorService y AsyncCalculatorService:

![image](https://user-images.githubusercontent.com/523243/145919482-9f4b4137-73e0-4668-928f-f93f12e939ba.png)

7. Haga clic en Listo. Verá que se crean dos nuevas clases de Apex en su organización con el nombre: - CalculatorService y AsyncCalculatorService.
8.  Para este ejercicio, nos centraremos en la clase CalculatorService.
9.  Ahora, echemos un vistazo al siguiente fragmento de código y probemos nuestra API ejecutándolo en la ventana de Apex anónimo en la consola del desarrollador.

![image](https://user-images.githubusercontent.com/523243/145919616-c94d0ea4-9e29-43ff-8611-88e1783e1237.png)

