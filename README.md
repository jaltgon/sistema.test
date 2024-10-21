# Configuración de Servidores DNS con BIND usando Vagrant

Este proyecto configura un servidor DNS maestro y un servidor DNS esclavo utilizando BIND en Debian a través de Vagrant. Se ha configurado el dominio `sistema.test` y se han implementado registros A, PTR, CNAME y MX. Además, se ha verificado la transferencia de zona entre el servidor maestro y el esclavo.

## Índice

- [Requisitos](#requisitos)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Configuración](#configuración)
- [Comprobaciones](#comprobaciones)
- [Licencia](#licencia)

## Requisitos

Para ejecutar este proyecto, necesitarás:

- [Vagrant](https://www.vagrantup.com/downloads) instalado.
- [VirtualBox](https://www.virtualbox.org/) instalado.
- Conocimientos básicos de terminal de Linux.

## Estructura del Proyecto

. ├── Vagrantfile ├── .gitignore ├── LICENSE ├── README.md ├── named.conf.options ├── named.conf.local ├── named.conf.options.slave ├── named.conf.local.slave ├── db.sistema.test ├── db.57.168.192 └── test.sh


- **Vagrantfile**: Archivo para configurar las máquinas virtuales.
- **.gitignore**: Ignora archivos innecesarios como `.vagrant/`.
- **LICENSE**: Archivo de licencia del proyecto.
- **README.md**: Documentación del proyecto.
- **named.conf.options**: Configuración de opciones del servidor maestro.
- **named.conf.local**: Configuración de la zona directa para el servidor maestro.
- **named.conf.options.slave**: Configuración de opciones del servidor esclavo.
- **named.conf.local.slave**: Configuración de la zona directa para el servidor esclavo.
- **db.sistema.test**: Archivo de la zona directa que contiene los registros A.
- **db.57.168.192**: Archivo de la zona inversa que contiene los registros PTR.
- **test.sh**: Script de prueba para validar la configuración.

## Configuración

1. Clona el repositorio en tu máquina local:

   ```bash
   git clone <URL_DEL_REPOSITORIO>
   cd <NOMBRE_DEL_REPOSITORIO>
   
2. Inicia las máquinas virtuales con Vagrant:

  vagrant up

3. Accede a la máquina del servidor maestro:

  vagrant ssh master

4. Configura el servidor DNS maestro y verifica que BIND esté funcionando:

  sudo systemctl status bind9

5. Accede al servidor esclavo:

  vagrant ssh slave

6. Verifica que la transferencia de zona se haya realizado correctamente.

##Comprobaciones

Para comprobar que la configuración DNS funciona correctamente, puedes usar los siguientes comandos:

-Comprobar registros A:

  dig @localhost sistema.test A

-Comprobar registros PTR:

  dig @localhost 192.168.57.103 -x

-Comprobar alias (CNAME):

  dig @localhost ns1.sistema.test

-Comprobar servidores NS:

  dig @localhost sistema.test NS

-Comprobar servidores MX:

  dig @localhost sistema.test MX

-Verificar transferencia de zona:

  dig AXFR sistema.test @192.168.57.103
