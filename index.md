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




