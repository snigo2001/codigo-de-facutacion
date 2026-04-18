Código de un sistema de facturación 
Introducción:
En el desarrollo de software moderno, la aplicación de patrones de arquitectura resulta fundamentales para garantizar la estabilidad, mantenibilidad y organización de los sistemas. Estos patrones permiten, estructura el código de manera más eficiente, facilitando la separación de responsabilidad y reduciendo el acoplamiento de componentes.
El presente documento tiene como objetivo el diseño e implementación de un sistemas de facturación desarrollado en Python, el cual permite la generar facturas en formato PDF con los datos ingresados en el sistema por el usuario.
Inicialmente, el sistema fue concebido bajo un enfoque cliente-servidor, sin embargo, durante su evolución se adoptó una arquitectura monolítica enriquecida con la implementación de diferentes patrones de arquitectura, como publicador-suscriptor, inyección de dependencia, arquitectura hexagonal y un enfoque conceptual de microservicios.
A través de este proceso, se busca evidenciar cómo la incorporación de estos patrones mejoras la organización  del código, permite una mayot  flexibilidad en el sistema y facilita en su futura escalabilidad.
Arquitectura general del sistema:
El sistema evolucionó desde una arquitectura cliente-servidor hacia una arquitectura de aplicación de escritorio monolítica, enriquecida con la implementación de patrones de diseño que mejoran la modularidad y el desacoplamiento del sistema.
La versión actual del sistema integra en un solo módulo las capas de presentación, lógica de negocio y persistencia, pero aplicando principios de separación de responsabilidades.
Caso de uso 
El sistema desarrollado correspondiente a un software de la facturación medica que permiten la generación de facturación en formato PDF a partir los datos ingresados.
El caso de uso principal es la creación de una factura, en el cual es el usuario ingresa la información del cliente y una lista de productos con sus respectivos valores y cantidades. El sistema procesa esta información se realizan los cálculos necesarios y genera automáticamente el documento PDF con los datos organizados.
Elementos del caso de uso: 
Actor: usuario
Entrada: nombre_cliente, productos, precios y cantidades 
Procesos: validación de datos, cálculos de subtotal, IVA y total
Salida: archivo PDF de la factura generado y almacenado local.
Requerimientos 
Requerimientos funcionales
•	El sistema debe permitir ingresar el nombre cliente
•	Agregar múltiples productos a una factura.
•	Validar que los datos ingresados sean correctos. 
•	Calcular automáticamente el subtotal, el IVA y el total.
•	Generar un archivo PDF con la información de la factura 
•	Almacenar el archivo PDF en una ruta específica del sistema.
•	Visualizar el PDF generado automáticamente 
Requerimientos no funcionales 
•	Ser fácil de usar mediante una interfaz gráfica.
•	Ejecutarse de manera local sin conexión a internet 
•	Generar los documentos en tiempos cortos.
•	Generar los documentos en tiempo cortos.
•	Ser mantenible y escalable.
•	Organizar los archivos generados de forma estructurada.
Detalles del código:
Servidor (server.py)
El servidor fue desarrollado utilizado Flask y cumple con las siguientes funciones:
Inicialización del servidor 
Se crea una aplicación web que permite manejar solicitudes HTTP. Además, se configuran las rutas para procesar las facturas. 
Gestión de archivos y almacenamiento 
El sistema define: 
•	Una carpeta llamada facturas_pdf donde se almacena los archivos pdf generados
•	Un archivo facturas.json que actúan como almacenamiento persistente de las facturas.
Si estos elementos no existen, el sistema los crea automáticamente.
Generación de id y fecha 
Cada factura generada incluye:
•	Un ID unico, basado en fecha y hora.
•	Una fecha de creación, registrada automáticamente.
Esto permite identificar cada factura de manera única.
Función de generación de PDF
El servidor utiliza una función encargada de:
•	Crea el documento PDF 
•	Insertar información: 
Titulo de factura 
Cliente
Fecha
Lista de productos
•	Calcular:
Subtotal 
iVA
total 
finalmente, el documento es guardado en la carpeta correspondiente.
Almacenamiento de datos 
Después de generar la factura:
Se guarda un registro en el archivo JSON con:
•	ID
•	Cliente 
•	Fecha
•	Ruta del PDF
Esto permite mantener un historial de facturas.
End ponit/factura 
Este endpoint:
•	Recibe datos desde el cliente mediante una solicitud POST 
•	Procesa la información 
•	Generar el PDF 
•	Guardan los datos
Retorna una respuesta en formato JSON con:
•	Estado de la operación
•	ID de la factura 
•	Ruta de archivo general
Permiten:
•	Consultar todas las facturas registradas.
•	Retornar el historial completo en formato JSON.
Cliente 
El cliente es una aplicación en consola que permite interactuar con el usuario.
Captura de datos 
El usuario ingresa:
•	Nombre del cliente 
•	Lista de productos, cada uno con:
Nombre 
Precio
Cantidad
Los productos se almacenan en una estructura tipo lista.
Envío de información
El cliente utiliza una solicitud HTTP POST para enviar los datos al servidor.
Recepción de respuesta 
El servidor responde con: 
•	Confirmación de la factura 
•	ID generado
•	Ruta del PDF
Esta información se muestra en consola al usuario.
Consulta de historia 
El cliente tambien permite :
•	Realizar una solicitud GET al servidor 
•	Obtener el listado de facturas
•	Mostrar el historial en pantalla
Flujo del sistema 
El funcionamiento completo del sistema es el siguiente:
1.	El usuario ingresa los datos en el cliente.
2.	El cliente envía los datos al servidor.
3.	El servido procesa la información.
4.	Se genera el PDF de la factura.
5.	Se guardan los datos en un archivo JSON
6.	El servidor responde al cliente 
7.	El cliente muestra el resultado
Tecnologías utilizadas 
•	Python
•	Flask
•	ReportLab 
•	Requests
•	JSON
Incorporación de patrones de arquitectura 
Durante el desarrolló de la versión final del sistema, se incorporo diferentes patrones de arquitectura para mejora la estructura del código.
Se implemento una clase encargada de gestionar eventos, permitidos la comunicación entre componentes sin depender directamente por la interfaz, sino como respuestas a un evento del sistema.
Adicionalmente, el generador de pdf fue encapsulado en componentes independientes, lo que permite aplicar el principio de inyecciones dependencia, facilitando la reutilización y el mantenimiento del sistema.
Publicador-suscripcritor 
Se implementó mediante la creación del sistema de eventos que permite desacoplar los componentes del sistema.
Cuando el usuario genera una factura, el sistema se publica un evento denominador “factura_creada”. Este evento es recibido por un suscritor encargado de ejecutar la creación de un archivo PDF.
Este enfoque elimina la dependencia directa entre la interfaz y la lógica de generación de documentos, permitiendo una arquitectura más flexible.
Inyección de demencias 
El sistema implementa el patrón de inyección al separar el generador de PDF como un componente independiste de la lógica principal. 
En lugar de crear directamente el generador dentro de las funciones, este es instanciado extremadamente y utilizados como una dependencia, primero modificarlo o cambiarlo sin afectar el sistema.
Microservicios  en forma conceptual
Aunque el sistema se ejecuta como  una aplicación local, se implementó una separación lógica, se implementó una separación lógica de componente que se simulaban de una arquitectura de microservicios.
Cada componente del sistema (interfaz, lógica de negocio y generación de PDF) opera de manera independientes, lo que permitiría en un futuro migrar cada uno como un servicio separado.
Arquitectura hexgonal 
El sistema  adopta un enfoque hexagonal, donde la lógica de negocios que se mantiene independientes las interfaces externas.
En esta interfaz grafica implementada con Tkinter actúa como adaptador de entrada. El generador de PDF actúa como adaptador de salida, la lógica de la facturación constituye el núcleo del sistema.
Este enfoque permite desacoplar el sistema de tecnologías especificas facilitando su  evolución y mantenimiento.
Historia de código:
Enlace github:
https://github.com/snigo2001/codigo-de-facutacion.git

V1:
Se arranco con prototipo simple usado Python como la biblioteca FLASK como tema del servidor y REPORTLAB para genera los PDF, reques se va como cliente aceptando las solicitudes del http del código, usado formado JSON para los datos siendo esta su versión 0.1 
Resultado:
 
Conclusiones: las funciones básicas funcionan menos el PDF, y también ejecutada como terminal.
V2:
Se cambio el enfoque de la arquitectura de cliente servidor a uno de por capas y mongólica.
Esto para agilizar los procesos y no extender el tiempo del desarrollo se quitó flask y todos los detalles de la arquitectura cliente servidor, para ser mongólica y por capas.
Detalle del código:
Capa de presentación:
Tkinter
 
•	Interacción con el usuario
•	Entrada de datos 
•	Mostar mensajes
Capa de lógica 
Funciones clave:
   
•	Validación de datos
•	Cálculo de subtotal,IVA y total
•	Organización de productos
Capas de presistencia 
 
•	Crear carpetas 
•	Guardar archivos PDF
•	Manejar rutas del sistema
Flujo del sistema 
 El usuario abre la aplicación. 
Ingresa los datos del cliente y productos. 
 Presiona el botón “Generar factura”. 
El sistema valida la información. 
 Se publica el evento “factura_creada”. 
 El suscriptor recibe el evento. 
Se genera el archivo PDF. 
• El archivo se guarda en el sistema. 
 El PDF se abre automáticamente.
Configuración inicial 
 
Genera ID únco 
 
Construye el contenido 
 
Calcula valores y genera el PDF
 
Resultados:
Interfaz de menú:
  
Interfaz de registro de factura :
 
PDF / factura:
 
Conclusión
El desarrollo del sistema permitió evidenciar la importancia de aplicar patrones de arquitectura en el diseño de software. A partir de una solución inicial simple, se logró evolucionar hacia una estructura más organizada y desacoplada.
La implementación del patrón Publicador–Suscriptor permitió mejorar la comunicación entre componentes, la Inyección de Dependencias facilitó la flexibilidad del sistema, y la Arquitectura Hexagonal permitió separar la lógica de negocio de las interfaces externas.
Estos cambios hacen que el sistema sea más mantenible, escalable y alineado con buenas prácticas de ingeniería de software.
Bibliografía 
•	Gamma, E., Helm, R., Johnson, R., & Vlissides, J. (1994). Design Patterns. 
•	Fowler, M. (2002). Patterns of Enterprise Application Architecture. 
•	Richardson, C. (2018). Microservices Patterns.
