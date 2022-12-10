---
layout: single
title: Apuntes de Linux
date: 2022-08-13
classes: wide
header:
  teaser: /assets/images/masthead.png
categories:
  - Linux
  - ApuntesUtiles
tags:
  - H4ch1rou
  - 
  -
---

## Apuntes de linux

Este tema fue escrito por el estudio del libro "**Command Line and Shell Scripting Bible**" Author "**Richard Blum**"

## Qué es linux?

Si tu nunca as trabajo con linux antes, puedes estar un poco confundido de que son o las diferentes versiones estan disponibles.

Para iniciar los sistemas linux se componen de 4 partes principales

- Kernel de linux
- Utilidades GNU
- Entorno grafico de escritorio
- Software de aplicación

## El sistema de Linux

![image-center](/assets/images/linux/linux_diagrama.jpg){: .align-center }

## Kernel

El nucleo de los sistemas linux es el **kernel**, este controla todo el hardware y software de un computador, asignado recursos del hardware cuando
es necesario, y ejecutando el software cuando es requerido.
**Linus torvalds** es el responsable de crear el primero kernel de linux mientras era estudiante en la universidad de helsinki, el intentaba realizar una copia de un sistema UNIX, que en ese tiempo era el sistema operativo mas usado en las universidades.

Despues de que linus desarrollara el kernel de linux, linus lo libera a la comunidad de internet y solicita sugerencias para mejorarlo, esta simple accion genera una revolucion  en el mundo de los sistemas operativos. Recibiendo sugerencias de estudiantes y profesionales de la programacion de todo el mundo.

Permitiendo a cualquiera cambiar el codigo del kernel que dio como resultado un completo caos, para simplificar las cosas , Linus actuo ciomo punto central para sugerencias de mejora. El tomando la decicion de que era lo que se incorporaba al kernel, acutlamente esto sigue en vigencia, pero ya no solamente linus realiza mejoras al kernel de linux, si no que es un equipo de desarrollo.

El kernel es responsable de 4 funciones principales

- Gestión de memoria del sistema
- Gestión de los programas
- Gestión del Hardware
- Gestión del sistema de archivos (Filesystem)

## Gestión memoria del sistema

Una de las principales funciones del kernel es el manejo de memoria, no solamente realizar el manejo de la memoria fisica disponible en el servidor, el puede crear y manejar memoria virtual o memoria que en realidad no existe.
Esto lo hace usando espacio del disco duro, llamado *espacio SWAP*, El Kernel intercambia los contenidos de ubicaciones de memoria virtual de ida y vuelta desde el espacio de intercambio hasta la memoria física real. Esto permite que el sistema piense que hay más memoria disponible de la que existe físicamente.

![image-center](/assets/images/linux/linux_kernelmemorymanagement.jpg){: .align-center }

Las localizaciones de memoria son agrupadas en bloques llamados **pages**, el kernel localiza cada page de memoria en cualquier lado de la memoria fisica o del espacio swap. El kernel mantiene en una tabla las pages de memoria indicando cual pagina esta en la memoria fisica y cual pagina esta en el swap de disco.  

El kernel sigue cual pagina de memoria esta en uso y automaticamente copia las paginas de memoria que no accesadas for un periodo de tiempo en el area de SWAP,
esto se llama **Swapping OUT** esto se realiza incluso incluso si existe memoria disponible. Cuando un programa quiere acceder a una Page de memoria que ha sido
intercambiado, el Kernel debe hacer espacio para él en la memoria física intercambiando una Page diferente de memoria e intercambiar la página requerida desde el espacio de intercambio. Obviamente, este proceso lleva tiempo, y puede ralentizar un proceso en ejecución. El proceso de intercambio de Page de memoria por la ejecución de aplicaciones continúa mientras el sistema Linux se esté ejecutando

tu puedes ver el estado acual de la memoria virtual en tu sistema linux en el archivo /proc/meminfo

```bash  
#!/bin/bash
 cat /proc/meminfo

  MemTotal:        5312060 kB   "#5 GB de memoria"
  MemFree:          569596 kB   "#500 MB de memoria libre"
  MemAvailable:    2563888 kB
  Buffers:          332200 kB
  Cached:          1523868 kB
  SwapCached:        10796 kB
  Active:          1061068 kB
  Inactive:        2985548 kB
  Active(anon):      31396 kB
  Inactive(anon):  2263096 kB
  Active(file):    1029672 kB
  Inactive(file):   722452 kB
  Unevictable:          16 kB
  Mlocked:              16 kB
  SwapTotal:        998396 kB
  SwapFree:         974432 kB
  Dirty:               200 kB
  Writeback:             0 kB
  AnonPages:       2152920 kB
  Mapped:           664996 kB
  Shmem:            103944 kB
  KReclaimable:     534872 kB
  Slab:             597504 kB
  SReclaimable:     534872 kB
  SUnreclaim:        62632 kB
  KernelStack:       12960 kB
  PageTables:        28124 kB
  NFS_Unstable:          0 kB
  Bounce:                0 kB
  WritebackTmp:          0 kB
  CommitLimit:     3654424 kB
  Committed_AS:    8164604 kB
  VmallocTotal:   34359738367 kB
  VmallocUsed:       45024 kB
  VmallocChunk:          0 kB
  Percpu:              888 kB
  HardwareCorrupted:     0 kB
  AnonHugePages:    696320 kB
  ShmemHugePages:        0 kB
  ShmemPmdMapped:        0 kB
  FileHugePages:         0 kB
  FilePmdMapped:         0 kB
  HugePages_Total:       0
  HugePages_Free:        0
  HugePages_Rsvd:        0
  HugePages_Surp:        0
  Hugepagesize:       2048 kB
  Hugetlb:               0 kB
  DirectMap4k:      190400 kB
  DirectMap2M:     5318656 kB
```

Por defecto cada proceso corriendo en los sistemas linux tiene su propio page de memoria, un proceso no puede acceder a una page de memoria que esta siendo utulizado por otro proceso. el kernel es el encargado de tener su areas de memoria. Por propositos de seguridad, ningun proceso puede usar la memoria usada por el kernel 

El comando especial **ipcs** muestra las paginas de memoria compartida en el sistema 

```bash
#!/bin/bash
ipcs -m 

---- Segmentos memoria compartida ----
key        shmid      propietario perms      bytes      nattch     estado      
0x00000000 32769      hachirou   600        524288     2          dest         
0x00000000 6          hachirou   600        8089600    2          dest         
0x00000000 8          hachirou   600        8089600    2          dest         
0x00000000 14         hachirou   606        2880000    2          dest         
0x00000000 15         hachirou   606        2880000    2          dest         
0x00000000 46         hachirou   600        524288     2          dest         
0x00000000 48         hachirou   600        57344      2          dest         
0x00000000 49         hachirou   600        57344      2          dest         
0x00000000 53         hachirou   600        286720     2          dest         
0x00000000 54         hachirou   600        286720     2          dest         
0x00000000 59         hachirou   606        10494720   2          dest         
0x00000000 60         hachirou   606        10494720   2          dest

```

Cada segmento de memoria compartida tiene un propietario que creó el segmento. Cada segmento también tiene una configuración de permisos estándar de Linux que establece la disponibilidad del segmento para los otros usuarios. El valor clave se utiliza para permitir que otros usuarios obtengan acceso al segmento de memoria compartida.

## Gestión de software

El sistema operativo linux llama a los programas en ejecucion *procesos*, un proceso puede correr en primer plano mostrando la salida por pantalla, o puede correr en segundo plano. el Kernel controla como el sistema linux administra todos los procesos corriendo en el sistema.
El Kernel crea el primer proceso llamado **Init Process**, para iniciar todos los otros procesos en el sistema. cuando el kernel inicia, este carga el init process dentro de la memoria virtual, como el kernel inicia cada proceso adicional, este tiene un unico area de memoria virtual para almacenar la data y el codigo que usa el proceso.
Algunas implementaciónes de linux contiene un tabla de proceso que inicia automaticamente al momento del booteo, en los sistemas linux esta tabla esta usualmente en el archivo /etc/inittabs.

**NOTA: Al parecer el archivo inittabs en los sistemas operativos linux actuales ya no existe**

Los sistemas operativos linux  usan un  sistema inicio que utiliza niveles de ejecución. un nivel de ejecucion puede ser usado directamente por el proceso de inicio para ejecutar solo ciertos tipos de procesos, definidos en el archivp /etc/inittabs, hay 5 niveles de niveles de ejecucion en el sistema operativo linux. 

En el nivel 1, solamente procesos de inicio basicos del sistema, junto con un proceso de consola terminal. Este es llamado modo de usuario unico, el modo de usuario unico es amenudo usado para mantencion de emergencia del sistema de archivo (filesystem) cuando este se rompe, obviamente este modo es solo para una persona ingresando en el sistema para manipular la data.
