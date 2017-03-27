# Tema 1

**1. Buscar información sobre las tareas o sitios web para los que se usan más los programas que comentamos al principio de la sesión (apache, nginx, thttpd, cherokee y nodejs).** 

  - **Apache:** es un servidor de web HTTP de código abierto para distintas plataformas como Unix o Microsoft Windows. Es utilizado para alojar servicios y páginas web estáticas y dinámicas. Apache es el componente de servidor web en la popular plataforma de aplicaciones LAMP, junto a MySQL y un gran número de lenguajes.
  - **nginx:** es un servidor web/proxy inverso ligero de alto rendimiento (se utiliza para equilibrar la carga entre los servidores de back-end, como también para ser utilizado como caché en un servidor de back-end lento) y un proxy para protocolos de correo electrónico con acceso al Internet Message Protocol (IMAP) y al servidor Post Office Protocol (POP).
Razones para usar nginx:
    1. Es ligero: reduce el consumo de RAM
    2. Es multiplataforma y fácil de instalar
    3. Se puede usar junto con apache 
    4. Se puede usar como caché, con algo de configuración, permitiendo mejorar la eficiencia de la aplicación.
    5. Se puede usar como balanceador de carga, distribuyendo el tráfico entre varios servidores, permitiendo mayor escalabilidad.
    6. Permite tener un soporte tanto profesional como comunitario.
    7. Es compatible con las aplicaciones web más populares como wordpress, drupal, etc.

    Conclusiones:
Tanto el consumo de recursos como la velocidad de respuesta al usuario son factores que influyen en los test de rendimiento de servidores web, y Nginx sabe cómo salir muy bien frente a Apache. Nginx a diferencia de Apache, no tiene módulos para servir contenido dinámico sea PHP, Python, Ruby, entre otros, para servir este contenido utiliza módulos externos que se pueden agregar al server, lo cual lo hace mucho más liviano y ágil.
Por tanto, Nginx se convierte en una opción más viable como reemplazo a Apache.
  - **thttpd:** es un servidor web de código libre disponible para la mayoría de variantes de Unix. Se caracteriza por ser simple, pequeño, portátil, rápido y seguro, ya que utiliza los requerimientos mínimos de un servidor HTTP.
El uso apropiado de esta herramienta es obtener velocidad en la transferencia de archivos y reducción de gastos innecesarios para funciones que no son requeridas en el servidor.
  - **Cherokee:** es un servidor web multiplataforma. Su objetivo es ser rápido y completamente funcional.
Algunas características son:
    - Soporta tecnologías como FastCGI, SCGI, PHP, CGI, SSI, SSL/TLS.
    - Soporta la configuración de servidores virtuales.
    - Permite la realización de redirecciones.
    - Permite su utilización como balanceador de carga
    - Dispone de un panel de autenticación.

  - **Nodejs:** es un entorno en tiempo de ejecución multiplataforma, de código abierto, para la capa del servidor. Está basado en el lenguaje de programación ECMAScript, asíncrono, con I/O de datos en una arquitectura orientada a eventos y basado en el motor V8 de Google. Fue creado con el enfoque de ser útil en la creación de programas de red altamente escalables como los servidores web.


