#Git Cheatsheet

 git init



Descargar un Repositorio


- Se crea una copia activa de un repositorio local utilizando el comando:


git clone /ruta/al/repositorio


- Cuando se usa un servidor remoto el comando es:

git clone username@host:/ruta/al/repositorio



Workflow


- El repositorio local consiste de tres 'arboles' mantenidos por Git. El primero es el directorio de trabajo, donde se encuentran los ficheros. Un segundo es el 'Index' que actúa como área de ensayo y finalmente la cabeza (head) que apunta al último commit que se ha lanzado.





Add and Commit


- Se pueden proponer ambios y añadirlos al Index con:


git add fichero | *


- Para guardar los ficheros en la 'Cabeza', pero no en el repositorio remoto:


git commit -m “mensaje”



Mandar al Repositorio Remoto


- En el paso anterior hemos dejado los cambios en la 'Cabeza', para mandar los cambios al repositorio remoto, se ejecuta:


git push origen maestro


- Maestro es la rama a la que queremos enviar los cambios


- Si no se tiene clonado un repositorio y queremos conectar nuestro repositorio al repositorio remoto:


git remote add origin Servidor


- De esta manera se pueden enviar los cambio al repositorio remoto seleccionado.



Ramificando


- Las ramas se usan para realizar modificaciones aisladas paralelas a la rama principal. La rama principal (default), es la que se crea al crear un repositorio.

- Las ramas aisladas para modificaciones paralelas, se vuelven a unir a la rama principal una vez completadas.

- Creamos una rama usando:


git checkout -b nombreRama


- Volvemos al maestro:


git checkout master


- Y borramos la rama que hemos creado:


git branch -d nombreRama


- Una rama no esta disponible para otros usuarios a menos que la subas al repositorio remoto:


git push origin nombreRama



Update and Merge


- Para actualizar el repositorio local a la última modificación:


git pull


- En el repositorio de trabajo para traer y fusionar cambios remotos.

- Para fusionar una rama con la rama activa:


git merge nombreRama


- en ambos casos git intenta auto-fusionar los cambios, y esto puede ocasionar conflictos. Somos responsables de fusionar manualmente esos conflictos en los ficheros que te indique git.

- Tras realizar los cambios, tienes que marcarlos como fusionados:


git add nombreFichero


- Antes de fusionar los cambios puedes comprobarlos utilizando:


git diff ramaOrigen ramaDestino



Etiquetado


- Se pueden etiquetar las versiones de software:


git tag 1.0.0 identificador


- el 1.0.0 se refiere a la versión.

- el identificador se refiere a los 10 primeros digitos del identificador de commit que queremos versionar.

- El identificador del commit se consigue:


git log



Logs


- Se puede ver el log del repositorio ejecutando:


git log


- Se puede modificar el log para que muestre los datos que queramos.

- Se puede uscar en el log por autor:


git log - -author=autor


- Para ver un log comprimido en el que cada commit es una linea


git log - -pretty=oneline


- Ver sólo aquellos ficheros que han cambiado


git log - -name-status


- La ayuda de log es:


git log - -help



Reemplazar Cambios Locales


- En caso de haber cometido un error se puede recuperar el contenido del último HEAD con:


git checkout – nombreFichero


- Los ficheros que hayan sido añadidos al INDEX y los ficheros nuevos, no se pierden.

- Si lo que deseas es coger la útima versión del servidor, para sustituir a la local:


git fetch origin

git reset - -hard origin/master



Puedes descargarte también este PDF:

[Git CheatSheet](http://rogerdudler.github.io/git-guide/files/git_cheat_sheet.pdf)
