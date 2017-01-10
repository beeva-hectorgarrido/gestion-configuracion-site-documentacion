#EN CONSTRUCCION

##Recomendaciones de ansible.com y el libro Ansible Playbook Essentials

Importante tener una buena organización por roles y basar los playbooks en ellos
Estrategia de organización de directorios y archivos. Por ejemplo:

  production                `#` inventory file for production servers
  staging                   # inventory file for staging environment

  group_vars/
     group1                 # here we assign variables to particular groups
     group2                 # ""
  host_vars/
     hostname1              # if systems need specific variables, put them here
     hostname2              # ""

  library/                  # if any custom modules, put them here (optional)
  filter_plugins/           # if any custom filter plugins, put them here (optional)

  site.yml                  # master playbook
  webservers.yml            # playbook for webserver tier
  dbservers.yml             # playbook for dbserver tier

  roles/
      common/               # this hierarchy represents a "role"
          tasks/            #
              main.yml      #  <-- tasks file can include smaller files if warranted
          handlers/         #
              main.yml      #  <-- handlers file
          templates/        #  <-- files for use with the template resource
              ntp.conf.j2   #  <------- templates end in .j2
          files/            #
              bar.txt       #  <-- files for use with the copy resource
              foo.sh        #  <-- script files for use with the script resource
          vars/             #
              main.yml      #  <-- variables associated with this role
          defaults/         #
              main.yml      #  <-- default lower priority variables for this role
          meta/             #
              main.yml      #  <-- role dependencies

      webtier/              # same kind of structure as "common" was above, done for the webtier role
      monitoring/           # ""
      fooapp/               # ""


#####Usar inventarios dinámicos si es posible

Si usas un proveedor cloud no deberías gestionar tu inventario con un fichero estático. No solo se aplica al cloud, si tienes otro sistema de mantenimiento de tu infraestructura, el uso de inventario dinámico es una buena opción.
Para el caso de cloud, nosotros usamos playbooks de integración entre la creación de la infraestructura y la instalación del software, que generan dinámicamente el inventario (ejemplo: aws_metadata.yml) , tratando así de independizar infraestructura y software
Usar variables de grupo y hosts
Podemos establecer unas variables para un cierto tipo de máquinas

`# file: group_vars/webservers`
apacheMaxRequestsPerChild: 3000
apacheMaxClients: 900

O unas variables por defecto para todos los grupos

# file: group_vars/all
ntp: ntp-boston.example.com
backup: backup-boston.example.com

Si usas inventarios dinámicos puedes utilizar etiquetas para inventariar las máquinas, por ejemplo en Amazon usar la etiqueta “class:webserver” cargaría las variables de entorno en "group_vars/ec2_tag_class_webserver".
Usa host_vars en caso de que haya excepciones en máquinas específicas.
Usa siempre un valor por defecto para las variables.
Variables y vaults
Cuando se ejecuta un playbook, Ansible encuentra las variables en el archivo no cifrado y todas las variables del archivo cifrado. Una buena práctica es dentro un grupo de group_vars crear un archivo vars y otro vault. Dentro de vars creamos todas las variables, incluso las que pueden ser sensibles.  Todas las variables sensibles las copiamos a archivo vault con el prefijo vault_. Después nos aseguramos de estas variables en vars apunten a las variables vault_ y que el archivo vault este cifrado.
En el caso de AWS utilizar AWS_PROFILE previo a la ejecución del playbook
Control de versiones
Use git o cualquier otro sistema de control de versiones. Es una buena manera de auditar las modificaciones realizadas.
Uso de tags
No es recomendable el abusar de tags ya que añaden complejidad.
Dependencias
Haz explícitas las dependencias de un playbook. Es preferible tener un buen listado de roles para poder ver todo lo que hay involucrado en nuestro playbook que tener que investigar para encontrar la información.
Comentarios
Ser generoso con los comentarios y los espacios en blanco. Ayuda a legibilidad y comprensión de los playbooks.
Usa siempre modulos disponibles
No uses el módulo command si ya hay un módulo equivalente
A menos que no cubra todas las opciones que el command pueda tener o no esté lo suficientemente maduro (docker)
Usar el parámetro state
Muchos módulos utilizan el parámetro state. Siempre trae un valor por defecto pero no dudes en setearlo siempre que puedas ya que dará información útil de lo que se pretende hacer.
Nomenclatura
Se debe utilizar una nomenclatura legible y estándar para toda la definición de las tareas, siendo esta propuesta de la siguiente manera:
“nombre role mayuscula”|”nombre tarea mayuscula”| “descripcion”
Ejemplo:
- name: AWS_LAMBDA | DEPLOY | Run creation of the lambda    

Utilización de bloques de código para el control de las excepciones, mediante el uso de “block” implementado a partir de la versión 2.0 de ansible, en el cual se pueden controlar las excepciones y asignar manejadores.
Utilización de organización de los parámetros de las tareas  en vertical para una mejor lectura del código, para una mejor comprensión ver el siguiente ejemplo.

p.e: 
debug: msg=”hello” verbosity=2

Si se aplica el code style que se menciona quedaria el codigo de la siguiente forma

debug:
  msg: “hello”
  verbosity: 2
-
Usar nombre en todas las tareas como se adjunta en el ejemplo.

Ejemplo:

- hosts: web tasks: 
- yum: name:
   httpd state: latest 
- service: 
   name: httpd 
   state: started 
   enabled: yes

Esto parece no ser correcto por lo que se deberia cambiar por lo siguiente.

- hosts: web 
  name: installs and starts apache tasks: 
- name: install apache packages 
  yum:
   name: httpd 
   state: latest 
- name: starts apache service 
   service:
    name: httpd 
    state: started 
    enabled: yes

Esto hace mas legible las tareas y mas trazables para ver cuales acciones en caso de error han fallado y poderlas localizar mas facilmente.

Divide y vencerás, esto es no crear mega playbooks, por lo que se deberian separar en varios playbook y distinguir las diferentes partes del stack como se ve en el ejemplo

acme_corp/ 
├── configure.yml 
├── provision.yml 
└── site.yml 
$ cat site.yml 
--- 
- include: provision.yml 
- include: configure.yml

Se distingue entre la parte de configuración y la parte de instalación haciendo mas mantenible el codigo.

Un módulo un repositorio git, esto hace mantenible el codigo del módulo sin interferir en los demás módulos 
Utilizar ansible-galaxy para la instalación de los módulos aka roles de ansible
Ejemplo


requirements.yml

# nodejs role
- src: https://github.com/nodesource/ansible-nodejs-role
  version: 592a239dca52e1c5ff79cfefd0072efe3831428e
  name: nodejs

 
