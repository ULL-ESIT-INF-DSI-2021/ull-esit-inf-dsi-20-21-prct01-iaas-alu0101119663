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



