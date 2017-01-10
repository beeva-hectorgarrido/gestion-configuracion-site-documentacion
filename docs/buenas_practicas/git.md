#BUENAS PRÁCTICAS: USO HERRAMIENTAS COLABORATIVAS - GIT

###1. Introducción

A través de este documento se va a intentar definir una serie de buenas prácticas a llevar a cabo entre los diferentes grupos de trabajo y sus integrantes. Estos grupos se encontrarán distribuidos en los diferentes departamentos de la empresa. 

Se tratará de estandarizar los procedimientos más adecuados para que la colaboración, revisión y edición de información sea lo más ágil y colaborativa posible, dentro de un orden, consistencia y de fácil uso por parte de todos los usuarios. 

###2. Descripción de Git

Git es un sistema de control de versiones distribuído. Un sistema de control de versiones permite la creación de un histórico para una colección de archivos y permite revertir la colección de archivos a otro estado anterior. Otro estado puede significar a otra colección diferente de archivos o contenido diferente de los archivos. 

Estos archivos generalmente son llamados "código fuente". En un sistema de control de versiones distribuído todos los colaboradores o usuarios con acceso al repositorio tienen una copia completa del código fuente y puede realizar operaciones referidas al control de versiones mediante esa copia local. Git mantiene todas las versiones, lo que facilita revertir a cualquier punto en la historia de tu código fuente usando Git.

Mediante Git se realizan los commits a tu repositorio local y es posible sincronizar ese repositorio con otros (tal vez remotos) repositorios. Git te permite clonar los repositorios, es decir, crear una copia exacta de un repositorio incluyendo la historia completa del código fuente. Los dueños de los repositorios pueden sincronizar los cambios vía push (transfiere los cambios al repositorio remoto) o pull (obtiene los cambios desde un repositorio remoto).

Git soporta el concepto de branching, es decir, se pueden tener variantes diferentes de un código fuente, de manera que se desea desarrollar una nueva característica, se puede abrir un branch o rama en tu código fuente y hacer los cambios en este branch sin afectar el principal desarrollo de tu código.

Git puede ser usado desde la línea de comandos o mediante interfaces gráficas, como es la que nos ocupa: GitLab



###3. Terminología propia de Git

Repositorio: Conjunto de código fuente que contiene toda la historia, las distintas versiones a lo largo del tiempo, los branchs y posibles etiquetas o tags. 

URL: Es el identificador que nos permite conocer el alojamiento del repositorio y nos sirve para poder apuntar a él.

Branch: Es una línea de desarrollo que surge a partir de una versión temporal del código fuente. Su principal aplicación es la de desarrollar nuevas funcionalidades o realizar cambios experimentales. Siempre debe de existir una original, comúnmente llamada “master”.

Commit: Cada vez que se realiza una modificación en el código fuente se crea un commit. De esta manera, todas las modificaciones temporales del repositorio quedan registradas y el control y evolución del repositorio es 100% identificable. 

Etiquetas: Es un recurso estético que nos permite apuntar a una determinada versión. En realidad es una friendly-name sobre un commit ID.



###4. Ejemplo de flujo de trabajo con Git


![Imagen obtenida de: http://nvie.com/posts/a-su_ccessful-git-branching-model/](http://nvie.com/img/git-model@2x.png)
	_Imagen obtenida de: http://nvie.com/posts/a-su_ccessful-git-branching-model/_




###5. Buenas prácticas a la hora de utilizar Git


#####_1. Crear flujos de trabajo por cada tarea_ 

Por cada tarea que llevemos a cabo, debemos abrir una rama haciendo referencia a la nueva feature que vamos a implementar. De esta manera permitimos desarrollar en paralelo distintas features, lo cual nos llevará a un desarrollo limpio y con posibilidad de decidir cuáles características nuevas incluir en la siguiente release y cuáles no. 

Este sistema también permite a distintos equipos desarrollar distintas releases de manera aséptica con los diferentes desarrollos que se estén haciendo en paralelo. 

#####_2. Probar antes de compartir_

Todos los cambios que se vayan haciendo han de ser probados por el desarrollador en su rama antes de publicar ningún tipo de cambio. Este modelo se asemeja al típico “prueba en local”. De esta manera el trabajo previo queda aislado y se aprovecha la posibilidad de trazabilidad y revertir modificaciones en caso de que la solución o la dirección del desarrollo no sea el correcto. Una vez que los cambios han sido probados, se puede compartir en ramas colaborativas y generales.

A parte de esta regla, es muy importante tener en cuenta la regla 9. 


#####_3. Probar todos los commits_

A partir de cualquier tipo de cambio que llevemos a cabo, debemos de realizar otra batería de pruebas antes de compartirlo. Esto que parece elemental nos puede evitar sustos a la hora de compartir cambios que con una batería de pruebas propia funcionan pero que no aguantan una batería de pruebas estandarizada por un equipo. 

Esta regla nos garantiza que cualquier desarrollo realizado en un repositorio es consistente con los requisitos mínimos de QA y rendimiento. 


#####_4. Hacer notas y comentarios clarificando los cambios realizados_

Deben de añadirse a los cambios notas y comentarios explicativos, clarificando la información.
Para el resto de equipos que lleguen a ver nuestros cambios, es de gran ayuda una explicación previa a lo que se va a llevar a cabo a posteriori

Para el formato de dichos comentarios podemos seguir las buenas prácticas dadas por cada proveedor, que nos da a nuestra disposición una serie de herramientas para llevar a cabo este tipo de tareas


#####_5. Despliegues automáticos_

Para agilizar el proceso de desarrollo y pruebas, los despliegues deben estar automatización para ahorrar el mayor tiempo posible.

Estos despliegues deben de llevarse a cabo en el momento que se detecte un  nuevo commit en el repositorio central. De forma que nuestro pipeline de automatización empiece en ese punto, con las pruebas unitarias, y el posterior despliegue en el entorno


#####_6. Realizar Snapshots_

Después de finalizar alguna nueva funcionalidad o de publicar nuevo código, es buena práctica realizar una snapshot del estado en el que se encuentra nuestro desarrollo.

De forma que diferenciemos entre 3 tipos de Snapshots:

Snapshot de desarrollo: Hace referencia a un nuevo desarrollo en nuestro flujo de trabajo

Candidates: Hace referencia a los cambios que se encuentran en los entornos de pruebas

Release-Candidate: Los cambios pendientes de hacerse públicos al resto de la comunidad


#####_7. Crear Release_

Cuando un nuevo desarrollo ha sido probado en flujos de trabajo previos y validado por el resto de desarrolladores. Se deben crear release para la publicación del nuevo código. Esto nos ayudará a agilizar una posible marcha atrás en caso de ser necesario. 


#####_8. Nunca rehacer algo publicado_

Cualquier desarrollo que llegue a ser público nunca debe ser reseteado y borrado.
Debe crearse un nuevo flujo para arreglarlo, de forma que pase las pruebas y validación del resto de desarrolladores


#####_9. Todo el mundo empieza de la rama Master_

Para que haya una referencia y uniformidad en los cambios. Todos los equipos deben partir de un mismo punto en común, que en nuestro caso será la rama Master.
Esto nos ayudará, a la hora de que todos los equipos estén actualizados con los últimos cambios se vayan haciendo públicos


#####_10. Mantener un orden a la hora de arreglar bugs en Releases_

Una de las peores prácticas que se pueden realizar es la de arreglar un bug en la rama de la release afectada. El orden correcto será desarrollar la solución en alguna rama específica para testing y una vez que la solución está consolidada, trasladarla a la rama master y desde esta propagar el cambio a las distintas releases (patch-xxx)


#####_11. Describir los cambios efectuados_

A la hora de realizar un commit sobre un repositorio, el incluir una buena descripción que indique qué cambios se han realizado y el porqué de los mismos es muy importante. 

Esta regla parece trivial, pero a la hora de enfrentarnos a un problema o tener que tomar la decisión de revertir un cambio o llevarlo a una rama de mayor importancia, una buena descripción de los cambios efectuados y el motivo de los mismos puede ayudarnos a tomar la decisión correcta o a resolver un posible problema o conflicto. 

_Documento escrito por Jesús Morales Aguiar y Francisco Moreno Fuentes_
