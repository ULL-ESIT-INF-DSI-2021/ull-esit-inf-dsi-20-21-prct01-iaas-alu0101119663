# Informe Práctica 1. Configuración de máquina virtual en el IaaS
## 1. Introducción

Durante este informe de la primera práctica de la asignatura **Desarrollo de Sistemas Informáticos**, se nos ha propuesto realizar la instalación y preparación de una máquina virtual alojada en el sistemas **Iaas** para un correcto desarrollo de la asignatura. También realizaremos comando para nuestra máquina local con el propósito de hacer que las conexiones sean rápidas entre ambas máquinas. Por último aprenderemos a usar **Markdown** y **Github Pages**.

## 2. Objectivos

El objetivo de esta práctica es la preparación del entorno de trabajo que utilizaremos durante esta asignatura, los principales objetivos dentro de la misma son: 

* Utilizar la **VPN** de la Universidad de La Laguna, así como acceder al entorno del Iaas.
* Realizar conexiones remonas con **SSH**.
* Configuración tanto de la máquina virtual como de la máquina local.
* Familiarización con las **claves SSH**.
* Configuración de un **prompt** de la máquina virtual.
* Instalación de **Git**.
* Instalación de **Node**.
* Familiarización con **Markdown** y con **Github Pages**.

## 3. Desarrollo de la práctica
### Tareas previas a la realización de la práctica 1

Antes de realizar la práctica debemos realizar algunas tareas previas, que son las siguientes

* Elegir grupo de trabajo.
* Cumplimentar la encuesta sobre expectativs y conocimientos previos.
* Darse de alta en **Google Classroom**.
* Tener una cuenta de **Github** asociada a nuestro **correo institucional**. Pudiendo solicitar los benificios que **Github** ofrece para estudiantes.
* Darse de alta en **Github Classroom**.
* Aceptar la asignación de Github Classroom.
* Leer y familiarizarnos con **Markdown**, **Github Pages** y **Jekyll**.

Una vez ya tengamos el repositorio para nuestra práctica, creamos un Github Pages, para hacerlo necesitamos ir a **Settings** en nuestro repositorio, bajamos hasta el apartado de **Github Pages**, escogemos la rama deseada en el apartado **Branch** y por último escogemos el tema que deseemos.

### 3.1 Configuración de la máquina virtual en el Iaas
#### 3.1.1 Conectarse a la VPN de la ULL

El primer paso que demos es conectarno a la VPN que nos ofrece la ULL, con esto podremos entrar al entorno del Iaas. Para conectarme, yo que utilizo Ubuntu uso la terminal con los siguientes comandos.

```bash
jaco@Sobremesa:~$ sudo vpnc-connect
[sudo] contraseña para jaco: 
Enter IPSec gateway address: vpn.ull.es
Enter IPSec ID for vpn.ull.es: ULL
Enter IPSec secret for ULL@vpn.ull.es: 
Enter username for vpn.ull.es: aluXXXXXXXXXX
Enter password for aluXXXXXXXXX@vpn.ull.es: 
```

Para saber los datos utilizados tanto en contraseñas como en ID he utilizado la información que nos proporciona la ULL.
[Configuración de VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/)

#### 3.1.2 Acceso al servicio IaaS de la ULL
Una vez nos hemos conectados a la **VPN** accedemos a la dirección web del [Iaas](https://iaas.ull.es). Allí entramos con nuestras credenciales de nuestra cuenta institucional. Elegimos la máquina virtual de **DSI** y la iniciamos, una vez iniciada vemos que se le asigna un nombre, en mi caso es **DIS-55**. En el apartado de **Interfaces de red** podremos ver la **IP** de nuestra máquina para así conectarnos por **SSH**.

#### 3.1.3 Conexión remota y primeras configuraciones
Estando conectados ya a la **VPN** y sabiendo a la **IP** que nos debemos conectar, abrimos una terminal para conectarnos a dicha **IP** de forma remota mediante **SSH**.

```bash
ssh usuario@10.6.XXX.XX
```

Una vez se haya conectado nos dará el siguiente mensaje

```bash
The authenticity of host '10.6.XXX.XXX (10.6.XXX.XXX)' can't be established.
...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
Debemos introducir ```yes```. Una vez hecho, nos pedirá la contraseña actual (usuario) y luego deberemos poner dos veces la contraseña que nosotros deseemos.

Lo siguiente que haremos es cambiar el nombre de la máquina virtual. Para realizar esta tarea abrimos el archivo ```/etc/hostname```.

```bash
usuario@ubuntu:~$ cat /etc/hostname 
ubuntu
usuario@ubuntu:~$ sudo vi /etc/hostname
[sudo] contraseña para usuario: 
usuario@ubuntu:~$ cat /etc/hostname 
iaas-dsi55
```
También deberemos cambiar el nombre antiguo de la máquina en el fichero ```/etc/hosts``` por el nuevo.

```bash
cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...

usuario@ubuntu:~$ sudo vi /etc/hosts
usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	iaas-dsi55
...
```

Ya que vamos a reiniciar la máquina aprovechamos para antes de hacerlo, instalar las actualizaciones de software con los siguientes comandos.

```bash
usuario@ubuntu:~$ sudo apt update
...
usuario@ubuntu:~$ sudo apt upgrade
...
```
Una vez han terminado ambos comandos, procedemos a reiniciar el sistemas.

```bash
usuario@ubuntu:~$ sudo reboot
```

Mientras esperamos que la máquina virtual termine de reiniciarse, podemos configurar el archivo ```/etc/hosts``` de la máquina local.

```bash
jaco@Sobremesa:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	Sobremesa

jaco@Sobremesa:~$ sudo vi /etc/hosts
[sudo] contraseña para jaco: 
jaco@Sobremesa:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	Sobremesa

10.6.129.14	iaas-dsi55
```

#### 3.1.4 Claves públicas-privadas
Las **claves SSH** son útiles a la hora de acceder a nuestra máquina virtual, ya que si la configuramos de un modo adecuado podemos entrar sin necesidad de estar poniendo la contraseña pertinente. Primero debemos tener una **clave ssh**, en mi caso ya la tenía creada pero en caso de que no fuera así se crearía con el siguiente comando:

```bash
jaco@Sobremesa:~$ ssh-keygen
```
Para generar la clave se pueden poner las opciones por defecto, es importante no introducir ninguna **passphrase** asociada al par de claves públicas-privada.

Como había dicho, ya yo tenía creada una clave. Se puede mostrar con  el siguiente comando:

```bash
jaco@Sobremesa:~$ cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCZk0oMZCi/DcPeNGN5nzilgK9GYdxKVZlvfYpJSQBc+w70mKjoOZ85KMpcrVAhiW+kVLAa7aKDCTjNwppWKKXWsdWHUWZfvPwTOv3CvdP+5B+TXp6IopzYCmG9yG0UeJkBjTRg5sWgdmzVGXHGG42pdlHvefGC8UWLNclQe/rHimVGoXeHhqKlRjiT90quqD9RwyFQef6l2Fd3hT+ZXs49oqOQxQiMCpe6y4WwIxTmTnie35ciPsWwt1suyK5c2A9yFM65KbMa2bIC+Dclzwpgu/MNR1N8d/0+EWnAzfTAwQYW4RJLFFiZ7Z0QIEJ2UdZCnOisJDpovIvi9v7eAgv5wZzoFYUz6IcFSk1viVybd1AZUja0F0uUtf7t17/seIq7KdoH0YlaPqYwQ6EmLk39SAO/whgIwiewI79tXxlqh34yjDcTwsvIVBfwB33ULTvQtzpgo2W645Vsj6J83PAhPQ95hsBOiQzeEklJkbX9eSnjUS1JPCJOyjV2rV77Xgk= jaco@Sobremesa
```
Una vez que la tengamos, ejecutamos el siguiente comando: 

```bash
jaco@Sobremesa:~$ ssh-copy-id usuario@iaas-dsi55
```
Esto permite que la clave se copia de la máquina local en la máquina virtual, esto hará que podemos conectarnos mediante **SSH** a la máquina virtual sin tener que preocuparnos por recordar la **IP** y la contraseña, ya que no la necesitaremos. Sólo con tener en la mente el nombre de la máquina bastaría.

```bash
jaco@Sobremesa:~$ ssh usuario@iaas-dsi55

jaco@Sobremesa:~$ ssh usuario@iaas-dsi55
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Feb 20 23:44:36 WET 2021
  ...
```

Para no tener que utilizar el nombre de usuario cada vez que querramos entrar a nuestra máquina virtual, podemos editar el fichero ```/.ssh/config```. (En caso de no tener el archivo creado se puede hacer con el comando ```touch```.

```bash
jaco@Sobremesa:~$ vi ~/.ssh/config
jaco@Sobremesa:~$ cat ~/.ssh/config
Host cya
  HostName 10.6.XXX.XXX
  User usuario

Host LPP
  HostName 10.6.XXX.XXX
  User usuario

Host iaas-dsi55
  HostName iaas-dsi55
  User usuario
```

Como resultado de haber editado este fichero, podremos realizar una conexión **SSH** tan sólo con indicar el nombre de la máquina.

```bash
jaco@Sobremesa:~$ ssh iaas-dsi55
```
Por último ya sólo nos queda generar las **claves pública-privada** en la **máquina virtual**. Este paso es idéntico al realizado para la **máquina local**

```bash
usuario@iaas-dsi55:~$ ssh-keygen
Generating public/private rsa key pair.
...
```
```bash
usuario@iaas-dsi55:~$ cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDevinjzkXTYSg5ai4F4FMqfSfQH1dlmVYyI5oR9PQAfJgM4yDdAfui6BtKWXEA+lmajFaQlBC178EDKsuVMhfHjYNByXypHQNDyAfoaV0Utl8C5Y3woGZ4Uu7noPu1tEdk7UaTY5uDrwtkFp19rF+FJ+fmyJLy+cYXXaTMqTfV3OdWUrBrDiEe144+EWseG72i1QbCUmcHThd7gmvcPN3cw3Iv8h/td+H7TE52jZxZrfBTflWdRz68fQ8qRJ5bJyffx4MfMCtgABTFjhiUE1lVyXylxn7lmR+lpkqj8W2TDhTi18h7UlVmyS1i1IeN11UoC/sZWsdMhK44vkXXyNZn usuario@iaas-dsi55
```
### 3.2 Instalación de git y Node.js en la máquina virtual del IaaS

#### 3.2.1 Instalación y configuración de git

Para instalar **Git** en nuestra **máquina virtual** utilizaremos el siguiente comando: 

```bash
usuario@iaas-dsi55:~$ sudo apt install git
```

Una vez se ha terminado de instalar seguimos con unas configuraciones iniciales, como se puede ver, son los siguientes: 

```bash
usuario@iaas-dsi55:~$ git config --global user.name "Jacobo Labrador"
usuario@iaas-dsi55:~$ git config --global user.email alu0101119663@ull.edu.es
usuario@iaas-dsi55:~$ git config --list
user.name=Jacobo Labrador
user.email=alu0101119663@ull.edu.es
```

#### 3.2.2 Configuración del prompt de la terminal de la máquina virtual
En este apartado haremos la configuración de un prompt de la terminal de la máquina virtual, esto no ayudará para saber en que rama actual nos encontramos cuando estemos en un determinado directorio que esté asociado a un **repositorio git**. Para ello debemos descargar el [script del prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh).
Otra opción es crear un fichero con dicho nombre (git-prompt.sh) mediante el comando ```touch```y mediante ```vi```abrirlo y copiar el contenido dentro.
Finalmente realizaremos la siguiente secuencias de comandos para modificar el fichero ```.bashrc``` y por último reiniciamos la terminal.

```bash
usuario@iaas-dsi55:~$ mv git-prompt.sh .git-prompt.sh
usuario@iaas-dsi55:~$ vi .bashrc
usuario@iaas-dsi55:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi55:~$ exec bash -l
[~()]$
```
#### 3.2.3 Conexión de la máquina virtual con Github.

Como se puede apreciar en la última línea del ejemplo anterior, el prompt ha cambiado. Pero aún debemos comprobar que funcione de forma correcta. Vamos a añadir la clave pública de nuestra máquina virtual en la configuración de las claves de nuestra cuenta de Github. Esto agilizará el trabajar con repositorios remotos, y en este caso podremos clonar un repositorio para hacer la prueba de que el **prompt** funcione correctamente.

```bash
[~()]$cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDevinjzkXTYSg5ai4F4FMqfSfQH1dlmVYyI5oR9PQAfJgM4yDdAfui6BtKWXEA+lmajFaQlBC178EDKsuVMhfHjYNByXypHQNDyAfoaV0Utl8C5Y3woGZ4Uu7noPu1tEdk7UaTY5uDrwtkFp19rF+FJ+fmyJLy+cYXXaTMqTfV3OdWUrBrDiEe144+EWseG72i1QbCUmcHThd7gmvcPN3cw3Iv8h/td+H7TE52jZxZrfBTflWdRz68fQ8qRJ5bJyffx4MfMCtgABTFjhiUE1lVyXylxn7lmR+lpkqj8W2TDhTi18h7UlVmyS1i1IeN11UoC/sZWsdMhK44vkXXyNZn usuario@iaas-dsi55
```
Una vez tengamos la clave pública copiada realizaremos lo que comenté en el apartado anterior. Vamos a nuestra cuenta de Github -> *Account Settings*. En la sección de *SSH and GPG keys*, pulsamos el botón de *New SSH key*, añadimos un título y pegamos la **clave pública** de nuestra máquina virtual en el campo que se nos indica. Pulsamos sobre *Add SSH key* para confirmar y si hemos hecho todo bien ya podríamos realizar clonaciones de repositorios.

```bash
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2021/prct01-iaas-vscode.git
Clonando en 'prct01-iaas-vscode'...
remote: Enumerating objects: 156, done.
remote: Counting objects: 100% (156/156), done.
remote: Compressing objects: 100% (117/117), done.
remote: Total 156 (delta 67), reused 85 (delta 34), pack-reused 0
Recibiendo objetos: 100% (156/156), 351.19 KiB | 1.02 MiB/s, listo.
Resolviendo deltas: 100% (67/67), listo.
```

```bash
[~()]$ls
prct01-iaas-vscode
[~()]$cd prct01-iaas-vscode/
[~/prct01-iaas-vscode(main)]$
```
Como se puede apreciar, hemos hecho todos los pasos correctamente ya que en el **prompt** se puede ver que estamos en la rama *main*.

#### 3.2.4 Instalación de Node
En este apartado instalaremos Node Version Manager (nvm), que es un gestor de versiones de **Node.js**. El cual es un entorno que permite la ejecución de código desarrollado en **JavaScript** y otros, como por ejemplo, **TypeScript**.
Para instalarlo haremos:

```bash
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
$exec bash -l
```
Ahora instalaremos la version más reciente de **Node.js**.

```bash
[~()]$nvm install node
...
```
Podemos comprobar las versiones de **Node** y de **NPM**.

```bash
[~()]$node --version
v15.9.0
[~()]$npm --version
7.5.3
```
Para instalar una **version concreta de NVM** podemos hacerlo de la siguiente forma:

```bash
[~()]$nvm install 12.0.0
...
```
Volvemos a comprobar las versiones de **Node** y de **NPM**.

```bash
[~()]$node --version
v12.0.0
[~()]$npm --version
6.9.0
```

Se puede comprobar que versión se está utilizando en estos momentos.

```bash
[~()]$nvm list
->      v12.0.0
        v15.9.0
default -> node (-> v15.9.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v15.9.0) (default)
stable -> 15.9 (-> v15.9.0) (default)
lts/* -> lts/fermium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.23.3 (-> N/A)
lts/erbium -> v12.20.2 (-> N/A)
lts/fermium -> v14.15.5 (-> N/A)
```

Para finalizar se puede cambiar de versiones de la siguiente forma.

```bash
[~()]$nvm use v15.9.0
Now using node v15.9.0 (npm v7.5.3)
```

Y si comprobamos podemos ver que el cambio se ha hecho de forma satisfactoria.

```bash
[~()]$node --version
v15.9.0
[~()]$npm --version
7.5.3
```
## 4. Conclusiones.
En conclusión, me parece que la realización de esta práctica siempre viene bien para refrescar conocimientos adquieridos. En mi caso ya estaba familiarizado a utilizar VPN y el entorno del Iaas debido a que se ha exigido en otras asignaturas. En cuanto a Git y Github, también se ha visto en otras asignaturas, pero nunca está de mas enfrentarse a una nueva configuración para asegurar conocimientos que ya tenemos. Sin embargo si he aprendido a configurar Node.js, nunca me había enfrentado a su instalación y por lo que he leído me resulta interesante. También por último reseñar que tanto el lenguaje de marcas, Markdown y las herramientas de Github Pages nunca había leído acerca de ellas ni tenía conocimiento de su existencia, por lo que me ha parecido muy interesante adquirir habilidad de con ellos para realizar buenas prácticas.
Para concluir creo que la dificultad no ha sido muy alta y no he tenido problemas a la hora de afrontar esta tarea.












