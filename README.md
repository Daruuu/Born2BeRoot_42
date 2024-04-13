# Born2BeRoot_42
Born2beroot es un proyecto de 42 Network que implica configurar un servidor virtual utilizando una distribución Linux, implementando medidas de seguridad como firewall y monitoreo de actividad para una mejora de protección de datos.
![VRlogo](https://techgenix.com/tgwordpress/wp-content/uploads/2022/03/1-1.png)

# Table of contents
- [Born2beRoot](https://github.com/daruuu/Born2BeRoot_42?tab=readme-ov-file#born2beroot)
- [What is LVM?](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#what-is-lvm)
- [The difference between aptitude and apt?](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#the-difference-between-aptitude-and-apt)
    - [Installing packages in aptitude and apt-get](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#installing-packages-in-aptitude-and-apt-get)
    - [Search for packages in aptitude and apt-get](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#search-for-packages-in-aptitude-and-apt-get)
    - [Remove packages in aptitude and apt-get](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#remove-packages-in-aptitude-and-apt-get)
- [AppArmor and SELinux](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#apparmor-and-selinux)
    - [SELinux](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#selinux)
    - [AppArmor](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#apparmor)
    - [The Difference between AppArmor and SELinux](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#the-difference-between-apparmor-and-selinux)
- [What is SSH?](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#what-is-ssh)
  - [How Does SSH Work?](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#how-does-ssh-work)
  - [Syntax of establishing an SSH Connection](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#syntax-of-establishing-an-ssh-connection)
- [What is UFW?](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#what-is-ufw)
    - [Let's deal with UFW](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#lets-deal-with-ufw)
    - [UFW Profiles](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#ufw-profiles)
- [User and Group Management](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#user-and-group-management)
  - [Users](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#users)
  - [Groups](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#groups)
- [Password Management](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#password-management)
  - [Password Policies](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#password-policies)
  - [Login Configuration](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#login-configuration)
- [SUDO](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#sudo)
  - [Understand SUDO](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#understand-sudo-super-user-do)
  - [Configure SUDO](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#configure-sudo)
- [Get close to crontab](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#get-close-to-crontab)
  - [How to use crontab](https://github.com/amaitou/Born2beRoot?tab=readme-ov-file#how-to-use-crontab)

# Born2beRoot

Este es el cuarto proyecto del cursus de 42 Network. <br />
El objetivo de este proyecto es ayudarte a configurar tu `Máquina virtual` bajo instrucciones específicas para acercarte y conocer más sobre el mundo de la virtualización.

El proyecto consta de dos partes
- **Parte obligatoria**
- **Parte de bonificación**

---

# ¿Qué es LVM?

**LVM** significa `Logical Volume Management/Manager`, es un sistema de gestión de almacenamiento de `Volúmenes Lógicos` (explicado más abajo).
**LVM** te ayuda a crear discos flexibles y te da la capacidad de administrarlos dinámicamente (redimensionamiento, rayado ...). <br />
**LVM** no trata con discos físicos, así que para crear tu `Volumen Lógico`, **LVM** convierte los discos físicos a `Volúmenes Físicos` luego los recopila en grupos llamados `Grupos de Volúmenes`, luego los entrega al `Volumen Lógico`.

* **Volumen físico** -> Un `Volumen Físico` es cualquier dispositivo de almacenamiento físico, como un disco duro (HDD), una unidad de estado sólido (SSD) o una partición, que se ha inicializado como un volumen físico con **LVM**, el `PV` es un trozo dividido de datos que también se conoce como `Extensiones Físicas` y que dura el mismo tamaño que las otras `PEs` (4 MB por defecto).

    ---

* **Grupo de volúmenes** -> El `Grupo de volúmenes` es un grupo de `Volúmenes Físicos` recopilados entre sí en un lugar llamado `VG`.

    ---

* **Volumen lógico** -> El `Volumen Lógico` es el resultado de la división de los `Grupos de Volúmenes`. en otras palabras, los `Grupos de Volúmenes` se vinculan entre sí en el `Volumen Lógico` que actúa como un **Disco Virtual**.

    ---

    **_Conclusión de LVM_**
    - `LVM` no trata con discos físicos.
    - cada Volumen Físico tiene varias `Extensiones Físicas`.
    - cada extensión tiene un tamaño específico (el tamaño predeterminado de `PE` es de _4 MO_).
    - Una sola `Extensión Física` es la unidad más pequeña de espacio en disco que puede ser administrada individualmente por `LVM`

    <br />

    **_Ejemplo_** <br />
    Tenemos un `Disco físico` con un tamaño de _500 GB_, y queremos convertirlo en _4_ `Volúmenes Físicos` con un tamaño de _125 GB_ para recopilarlos dentro de un `Grupo de Volúmenes`. <br />
    Así es como se calcula el número de `Extensiones Físicas` (El tamaño predeterminado es 4 MO): <br />

    - primero, conozcamos cuántas PEs habría en 1 GB: <br />
        `1 024 / 4 = 256` <br />
    - multiplique el resultado anterior por el tamaño de cada PV para darnos cuántas PEs habría dentro de un PV: <br />
        `125 * 256 = 32 000` <br />
    - multiplique el resultado de la operación anterior por 4 ya que tenemos 4 PVs: <br />
        `32 000 * 4 = 128 000` <br />

    Cada `Volumen Físico` tendría _32 000_ `PEs` y el total de `PEs` de los `PVs` recopilados es _128 000_.

---

# La diferencia entre **aptitude** y **apt**?

Tanto `apt-get` como `aptitude` son gestores de paquetes responsables de cualquier actividad relacionada con paquetes en sistemas Linux. La diferencia más notable es que `aptitude` ofrece una interfaz de menú en la terminal, mientras que `apt-get` no. Aunque son similares, `aptitude` ofrece algunas características adicionales:

- `aptitude` elimina automáticamente los archivos relacionados con un paquete al desinstalarlo, mientras que en `apt-get` se requiere un comando específico para hacerlo.
- `aptitude` no solo realiza las funciones de `apt-get`, sino que también maneja algunas herramientas complementarias como `apt-cache` y `apt-mark`.
- En caso de conflictos, `aptitude` sugiere varias resoluciones posibles, mientras que `apt-get` simplemente informa que no puede realizar la acción.
- `aptitude` tiene comandos como `por qué` y `por qué no` que explican por qué ciertos paquetes instalados manualmente están evitando una acción.
- Aptitude puede determinar la razón para instalar un paquete buscando en la lista de paquetes instalados y verificando si alguna de sus dependencias sugiere ese paquete.

En general, la sintaxis de `aptitude` es similar a la de `apt-get`, lo que facilita la transición para los usuarios de este último. Sin embargo, `aptitude` ofrece características adicionales que pueden hacerlo más preferible en ciertos casos.


### **Instalación de paquetes en `aptitude` y `apt-get`**
```sh
# apt-get
apt-get install <NombrePaquete>

#aptitude
aptitude install <NombrePaquete>
