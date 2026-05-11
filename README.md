
# 

# 

# 

#  **Propuesta Técnica de Implantación** 

**Módulo**:Lenguaje de Marcas  
**Profesor:**Willman Acosta Lugo  
**Alumnos**: Lucas Moreno Bravo  
	    Jose Luis Segura Fernández  
	    Roberto Heredia Cháves  
**Fecha:**11/05/2026

## Bloque A: Análisis de Mercado y Selección (CE a, c)

Elegimos Odoo porque es más equilibrado para un pyme de 25 empleados que quiere crecer y automatizarlo todo.

Todos los departamentos trabajan sobre una misma base de datos, con información centralizada, actualizada y accesible en tiempo real.  
Las tareas repetitivas se reducen, los flujos se ordenan y el equipo gana tiempo para centrarse en actividades de mayor valor. Por lo que es perfecto para este tipo de empresas aunque con un coste elevado de unos 17.9 por usuario al mes lo que en nuestra empresa sería 447.5€/mes que al año sería 5370€/año según la página oficial de ERP para empresas [Enlace](https://go.processcontrol.com/erp-para-empresas/?utm_source=google&utm_medium=cpc&utm_campaign=erp-wp&utm_id=odoo&gad_source=1&gad_campaignid=23797734174&gbraid=0AAAAABnkaG37598K4hMLNAMCINK_7jL_J&gclid=Cj0KCQjw_IXQBhCkARIsADqELbJai_jKG5-25D-cx3J_o_iKSVpW6buvPHaR-EpXI6v3Vf592dzFBsUaAonkEALw_wcB).

**2\. Cálculo de TCO:** ESTE PRESUPUESTO DE SUSCRIPCIONES ESTA REALIZADO PARA LOS GASTOS DE 3 AÑOS 

**Coste de licencias/suscripción.**

Suscripción a odoo: Odoo Enterprise 17,90 por persona : 16110 euros por 3 años 

**Coste de implantación (vuestras horas de desarrollo: estima 100h a 40€/h).**

Horas de trabajo totales : 5000

**Coste operativo (Hosting en Google Cloud, AWS, Huawei Cloud o similar).**

$40.00 USD/mes: 8 GB RAM, 2 vCPU, 160 GB SSD, 1 TB Transferencia  
Hosting 1440 euros Los 3 años

**Lo que supondría para la empresa mensualmente :** 626 euros de gasto mensual la implementación de sistema de gestión empresarial.

Bloque B: Diseño de Seguridad RBAC

Diseño de matriz de permisos para Administrador, Comercial, operario de almacen y contable 

| Objeto | Administrador | Comercial | Operario de almacen | Contable |
| ----- | ----- | ----- | ----- | ----- |
| Clientes | CRUD | lectura | sin acceso | lectura |
| Presupuestos | CRUD | lectura | sin acceso | sin acceso |
| Stock | CRUD | sin acceso | lectura | solo lectura |
| Albaranes Entrada/Salida | CRUD | sin acceso | lectura | sin acceso |
| Facturas | CRUD | sin acceso | lectura | lectura y escritura |

## Bloque C: Documentación de Explotación (CE i)

Siguiendo la norma **ISO/IEC 26514**, redacta un breve **Manual de Despliegue** para que el responsable de IT de la empresa pueda levantar el sistema en caso de caída. Debe incluir:

1. El fragmento de *docker-compose.yml* necesario.

services:

  odoo:

    image: odoo:latest

    container\_name: odoo

    restart: unless-stopped

    depends\_on:

      \- db

    ports:

      \- "8200:8069"

    volumes:

      \- odoo-data:/var/lib/odoo

      \- ./config:/etc/odoo

      \- ./addons:/mnt/extra-addons

    environment:

      \- HOST=db

      \- USER=odoo

      \- PASSWORD=odoo

    command: odoo \-d odoo \--db\_user=odoo \--db\_password=odoo \-i base

  db:

    image: postgres:16.0

    container\_name: db

    restart: unless-stopped

    environment:

      \- POSTGRES\_DB=odoo

      \- POSTGRES\_PASSWORD=odoo

      \- POSTGRES\_USER=odoo

      \- PGDATA=/var/lib/postgresql/data/pgdata

    volumes:

      \- db-data:/var/lib/postgresql/data

volumes:

  odoo-data:

  db-data:

2. El comando para realizar un backup de la base de datos PostgreSQL.

El comando para el backup en la última versión es:pg\_dump.exe \-h localhost \-p 5432 \-U postgres \-F c \-v \-d {nombre\_de\_tu\_db} \-f {ruta\_donde\_se\_aloja\_el\_archivo} 

# ¿Es coherente el TCO con la realidad de una PYME?

Para una pyme del tamaño de la nuestra 25 personas gastarse 628 euros al mes por el sistema de gestion empresarial es una cifra muy coherente y competitiva ya que se ahorran un tiempo 
de unas 2 horas al mes por trabajador lo que multiplicado por 25 son 50 euros al mes que la empresa gana de tiempo para sus getiones y ganacias lo que es muy rentable y que el sistema son uno 
25 euros por elmpleado 


# ¿La matriz RBAC evita que el comercial vea los costes de producción?

No lo evita ya que el comercial al tener acceso a lectura de Clientes y Presupuestos podra visionar costes de los productos, si en algun momento hay algo que no deba de ver el administrador cambiara los permisos del comercial o se ocultaria en otro lugar al que no tenga acceso el comercial. A no ser que esa informacion este en el stock o en los albaranes el comercial podra ver los costes de produccion.

# ¿El comando de backup es sintácticamente correcto?
No, no es correcto, el comando crrecto es "pg_dump.exe -h localhost -p 5432 -U postgres -F c -v -d {nombre_de_tu_db} -f {ruta_donde_se_aloja_el_archivo}" lo que es muy largo y complicdo de memorizar , en otras versiones existe este comando "pg_dump -U <usuario> <nombre_bd>", pero OJO! no tienes que hacerlo en cualquier sitio, debes hacerlo en el cmd de Windows, si no te dará error, Según un foro de Internet [Visita el foro](https://es.stackoverflow.com/questions/5195/c%C3%B3mo-hacer-un-backup-de-una-base-de-datos-postgres-en-windows)
