### Escuela Colombiana de Ingeniería
### Arquitecturas de Software - ARSW

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/en-us/free/search/?&ef_id=Cj0KCQiA2ITuBRDkARIsAMK9Q7MuvuTqIfK15LWfaM7bLL_QsBbC5XhJJezUbcfx-qAnfPjH568chTMaAkAsEALw_wcB:G:s&OCID=AID2000068_SEM_alOkB9ZE&MarinID=alOkB9ZE_368060503322_%2Bazure_b_c__79187603991_kwd-23159435208&lnkd=Google_Azure_Brand&dclid=CjgKEAiA2ITuBRDchty8lqPlzS4SJAC3x4k1mAxU7XNhWdOSESfffUnMNjLWcAIuikQnj3C4U8xRG_D_BwE). Al hacerlo usted contará con $200 USD para gastar durante 1 mes.

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Escalabilidad Serverless (Functions)

1. Cree una Function App tal cual como se muestra en las  imagenes.

![](images/part3/part3-function-config.png)

![](images/part3/part3-function-configii.png)

2. Instale la extensión de **Azure Functions** para Visual Studio Code.

![](images/part3/part3-install-extension.png)

3. Despliegue la Function de Fibonacci a Azure usando Visual Studio Code. La primera vez que lo haga se le va a pedir autenticarse, siga las instrucciones.

![](images/part3/part3-deploy-function-1.png)

![](images/part3/part3-deploy-function-2.png)

4. Dirijase al portal de Azure y pruebe la function.

![](images/part3/part3-test-function.png)

5. Modifique la coleción de POSTMAN con NEWMAN de tal forma que pueda enviar 10 peticiones concurrentes. Verifique los resultados y presente un informe.

6. Cree una nueva Function que resuleva el problema de Fibonacci pero esta vez utilice un enfoque recursivo con memoization. Pruebe la función varias veces, después no haga nada por al menos 5 minutos. Pruebe la función de nuevo con los valores anteriores. ¿Cuál es el comportamiento?.

**Preguntas**

* ¿Qué es un Azure Function?
 Es una solución para ejecutar fácilmente pequeños fragmentos de código o “funciones” en la nube
 Ventajas del uso de Azure Function.

    * Podemos codificar todo el código que necesitemos para el problema / acción que se quiere ejecutar sin preocuparnos de la aplicación o la infraestructura para ejecutarlo.
    * Hace que el desarrollo sea más productivo.
    * Cambio en el modelo de pago frente a los WebJobs. Azure Function dispone de dos modelos de pago y, además, se puede incluir en un modelo de pago ya existente como un App Service. También se puede pagar solo por el tiempo que este en ejecución esa función.
    * Podemos codificar en diferentes lenguajes de programación, como C#, F#, Node.js, Java o PHP.
    * Nos permite desarrollar aplicaciones sin servidor en Microsoft Azure.
* ¿Qué es serverless?
Es un modelo de ejecución en el que el proveedor de la nube (AWS, Azure o Google Cloud) es responsable de ejecutar un fragmento de código mediante la asignación dinámica de los recursos. Y solo cobrando por la cantidad de recursos utilizados para ejecutar el código. El código generalmente se ejecuta dentro de contenedores sin estado que pueden ser activados por una variedad de eventos que incluyen solicitudes http, eventos de bases de datos, servicios de cola, alertas de monitoreo, cargas de archivos, eventos programados (trabajos cron), etc.
* ¿Qué es el runtime y que implica seleccionarlo al momento de crear el Function App?
El runtime es el intervalo de tiempo en el que un programa permanece en ejecucion. En este laboratorio, se utilizo el plan Consumption y la versión del tiempo de ejecución 12 LTS , que significa que el tiempo de espera será de 5 minutos y la memoria se limpiará una vez trasncurrido dicho intervalo de tiempo.
* ¿Por qué es necesario crear un Storage Account de la mano de un Function App?
Azure Storage contiene todos los objetos de datos de Azure Storage: blobs, archivos, colas, tablas y discos. La cuenta de almacenamiento proporciona un espacio de nombres único para los datos de Azure Storage que es accesible desde cualquier lugar del mundo a través de HTTP o HTTPS. Los datos de la cuenta de Azure Storage son duraderos y altamente disponibles, seguros y escalables a gran escala.
* ¿Cuáles son los tipos de planes para un Function App?, ¿En qué se diferencias?, mencione ventajas y desventajas de cada uno de ellos.
Azure function nos proporciona tres planes de beneficios.
Consumption plan:
✔ Plan de alojamiento
predeterminado.
✔ Pagar solo cuando se ejecuten las funciones.
✔ Escala automáticamente, incluso durante períodos de alta carga.
Plan premium: 
✔ Tiene un gran número de ejecuciones pequeñas y una factura de ejecución alta, pero pocos segundos GB en el plan de consumo.
✔ Necesita más opciones de CPU o memoria que las proporcionadas por el plan de consumo.
✔ el código debe ejecutarse más tiempo que el tiempo máximo de ejecución permitido en el plan de consumo.
✔ Necesita características que no estén disponibles en el plan de consumo, como la conectividad de red virtual.
Plan dedicado:
✔ Tiene máquinas virtuales existentes y subutilizadas que ya están ejecutando otras instancias de App Service.
✔ Desea proporcionar una imagen personalizada en la que ejecutar las funciones.
✔ Se requiere escalado predictivo y costes.
![](images/part3/foto1.png)
* ¿Por qué la memoization falla o no funciona de forma correcta?
* ¿Cómo funciona el sistema de facturación de las Function App?
Las facturas de Azure Functions se basan en el consumo de recursos y las ejecuciones por segundo. Los precios del plan de consumo incluyen 1 millón de solicitudes y 400.000 GB de consumo gratuito de recursos al mes; después de eso, la facturación se basa en el consumo de recursos medido en GB. El consumo de recursos se calcula multiplicando el tamaño medio de la memoria en GB por el tiempo en milisegundos que se tarda en completar la tarea. La cantidad de memoria utilizada por una función se mide pasando de 128 MB a un máximo de 1.536 MB, y el tiempo de ejecución se mide pasando de 1 ms a 1 ms. El tiempo mínimo de ejecución para una sola función es de 100 milisegundos y la memoria mínima es de 128 megabytes.
* Informe
