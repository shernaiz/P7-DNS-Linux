# P7-DNS-Linux

Configurar un sistema cun servidor DNS e un cliente alpine que cumpla os seguintes requisitos:
1. Volumen por separado da configuracion
2. Red propia interna para tódo-los contenedores
3. IP fixa no servidor
4. Configurar Forweders
5. Crear Zona propia (Rexistros a configurar: NS, A, CNAME, TXT, SOA)
6. Cliente con ferramentas de rede

# README
**1. Explicación das liñas de docker-compose.**
**2. Procedemento de creación de servicios(contenedor).**
**3. Modificación da configuración, arranque e parada do servicio bind9.**
**4. Configuración da zoa e cómo comprobar que funciona.**

# Explicación do `compose.yml`

O primeiro que teremos que definir no compose serían os dous contenedores e logo definiremos a rede.
Primeiro definimos o servizo que fará de servidor DNS.

**Servidor** -> Os parámetros que teremos que definir do servidor serán:
1. A *imaxe* que usaremos: neste caso a de bind9

2. Os *portos* que abriremos para recibir as peticións: neste caso o 54 xa que usamos máquina virtual.

3. A *rede* á que se conectará o contenedor: que será a que definimos ao final do yml.

4. Os *volumes* nos que se montará a configuración:  ainda que estos volumes soen facerse na carpeta /etc/bind, fixenos nunha carpeta dentro da persoal para evitar problemas de permisos.

**Cliente** Neste usaremos os mismos parámetros que o anterior pero quitando algúns, principalmente definiremos os seguintes:

1. A *imaxe*: que para o cliente será a de alpine.

2. O *stdin*: esto permitirá a entrada estándar no contenedor.

3. O *DNS*: que indicará o servidor DNS ao que nos conectamos, neste caso teremos que poñerlle a dirección IP do outro ontenedor, o que actúa de servidor.

4. A *rede*: que, como no contenedor anterior, indicará a rede a que se conecta o contenedor.

**Networks** Neste apartado definiremos a rede que se creará para que os contenedores definidos no .yml se conecten a esta.

1. o *nome* da rede: permitiranos atopala mais facilmente.

2. O *rango* de IPs: nas que se manexa a rede

3. A *gateway*: que será a dirección por defecto da rede.

**Explicación dos volumes**
Para que o servidor DNS funcione temos que crear algúns volumes onde aloxar os archivos de configuración deste, principalmente crearemos dúas carpetas:

1. *Config*: Nesta carpeta teremos que montar os archivos de configuración ou de datos específicos que o contenedor precisará para funcionar correctamente. 

2. *Zonas*: Nesta carpeta teremos que añadir os arquivos de Zonas, que serán os que definen os datos que se consultan. Nesta definimos as IP e a información referente e enecesaria para o contenedor que fará de servidor DNS.

Cando teñamos todos os volumes e o .yml fitos correctamente, desde a carpeta xa podremos lanzar o comando `docker compose up` e funcionaría correctamente. 

Para comprobar que funciona podremos entrar dentro do contenedor (`docker exec -it servidor_bind sh`) e probar a face un dig á dirección do DNS ou a zona directamente ("example.com") e tenos que dar resposta ca información dos ervidor que metimos nos arquivos de configuración