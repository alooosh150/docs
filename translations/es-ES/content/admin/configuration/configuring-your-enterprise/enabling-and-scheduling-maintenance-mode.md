---
title: Habilitar y programar el modo de mantenimiento
intro: 'Algunos procedimientos de mantenimiento estándar, como la actualización {% data variables.product.product_location %} o la restauración de copias de seguridad, exigen que la instancia esté sin conexión para el uso normal.'
redirect_from:
  - /enterprise/admin/maintenance-mode
  - /enterprise/admin/categories/maintenance-mode
  - /enterprise/admin/articles/maintenance-mode
  - /enterprise/admin/articles/enabling-maintenance-mode
  - /enterprise/admin/articles/disabling-maintenance-mode
  - /enterprise/admin/guides/installation/maintenance-mode
  - /enterprise/admin/installation/enabling-and-scheduling-maintenance-mode
  - /enterprise/admin/configuration/enabling-and-scheduling-maintenance-mode
  - /admin/configuration/enabling-and-scheduling-maintenance-mode
versions:
  ghes: '*'
type: how_to
topics:
  - Enterprise
  - Fundamentals
  - Maintenance
  - Upgrades
shortTitle: Configurar el modo de mantenimiento
---

## Acerca del modo de mantenimiento

Algunos tipos de operaciones exigen que desconectes tu {% data variables.product.product_location %} y la pongas en modo de mantenimiento:
- Actualizar a una versión nueva de tu {% data variables.product.prodname_ghe_server %}
- Aumentar los recursos de CPU, memoria o almacenamiento asignados a la máquina virtual
- Migrar datos desde una máquina virtual a otra
- Restaurar datos desde una instantánea de {% data variables.product.prodname_enterprise_backup_utilities %}
- Solucionar ciertos tipos de problemas críticos de solicitud

Recomendamos que programe una ventana de mantenimiento para, al menos, los siguientes 30 minutos para darle a los usuarios tiempo para prepararse. Cuando está programada una ventana de mantenimiento, todos los usuarios verán un mensaje emergente al acceder al sitio.



![Mensaje emergente para el usuario final acerca del mantenimiento programado](/assets/images/enterprise/maintenance/maintenance-scheduled.png)

Cuando la instancia está en modo de mantenimiento, se rechazan todos los accesos HTTP y Git. Las operaciones de extracción, clonación y subida de Git también se rechazan con un mensaje de error que indica que temporalmente el sitio no se encuentra disponible. Se pausará la replicación de git en las configuraciones de disponibilidad alta. No se ejecutarán los jobs de las Github Actions. Al visitar el sitio desde un navegador aparece una página de mantenimiento.

![La pantalla de presentación del modo de mantenimiento](/assets/images/enterprise/maintenance/maintenance-mode-maintenance-page.png)

{% if ip-exception-list %}

Puedes llevar a cabo una validación inicial de tu operación de mantenimiento si configuras una lista de IP de excepción para permitir el acceso a {% data variables.product.product_location %} solo desde las direcciones IP y rangos de ellas que proporcionaste. Los intentos para acceder a {% data variables.product.product_location %} desde las direcciones IP que no se especifican en la lista de excepciones IP recibirán una respuesta consistente con aquellas enviadas cuando la instancia esté en modo de mantenimiento.

{% endif %}

## Habilitar el modo de mantenimiento de inmediato o programar una ventana de mantenimiento para más tarde

{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
2. En la parte superior de la {% data variables.enterprise.management_console %}, haz clic en **Mantenimiento**. ![Pestaña de mantenimiento](/assets/images/enterprise/management-console/maintenance-tab.png)
3. En "Habilitar y Programar", decide si habilitas el modo de mantenimiento de inmediato o programas una ventana de mantenimiento para otro momento.
    - Para habilitar el modo de mantenimiento de inmediato, usa el menú desplegable y haz clic en **now** (ahora). ![Menú desplegable con la opción para habilitar el modo de mantenimiento ahora seleccionado](/assets/images/enterprise/maintenance/enable-maintenance-mode-now.png)
    - Para programar una ventana de mantenimiento para otro momento, usa el menú desplegable y haz clic en un horario de inicio. ![Menú desplegable con la opción para programar una ventana de mantenimiento](/assets/images/enterprise/maintenance/schedule-maintenance-mode-two-hours.png)
4. Selecciona **Habilitar el modo de mantenimiento**. ![Casilla de verificación para habilitar o programar el modo de mantenimiento](/assets/images/enterprise/maintenance/enable-maintenance-mode-checkbox.png)
{% data reusables.enterprise_management_console.save-settings %}

{% if ip-exception-list %}

## Validating changes in maintenance mode using the IP exception list

The IP exception list provides controlled and restricted access to {% data variables.product.product_location %}, which is ideal for initial validation of server health following a maintenance operation. Once enabled, {% data variables.product.product_location %} will be taken out of maintenance mode and available only to the configured IP addresses. The maintenance mode checkbox will be updated to reflect the change in state.

If you re-enable maintenance mode, the IP exception list will be disabled and {% data variables.product.product_location %} will return to maintenance mode. If you just disable the IP exception list, {% data variables.product.product_location %} will return to normal operation.

{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
1. At the top of the {% data variables.enterprise.management_console %}, click **Maintenance**, and confirm maintenance mode is already enabled. ![Pestaña de mantenimiento](/assets/images/enterprise/management-console/maintenance-tab.png)
1. Select **Enable IP exception list**. ![Checkbox for enabling ip exception list](/assets/images/enterprise/maintenance/enable-ip-exception-list.png)
1. In the text box, type a valid list of space-separated IP addresses or CIDR blocks that should be allowed to access {% data variables.product.product_location %}. ![completed field for IP addresses](/assets/images/enterprise/maintenance/ip-exception-list-ip-addresses.png)
1. Haz clic en **Save ** (guardar). ![after IP excetpion list has saved](/assets/images/enterprise/maintenance/ip-exception-save.png)

{% endif %}

## Programar el modo de mantenimiento con {% data variables.product.prodname_enterprise_api %}

Puedes programar el mantenimiento para horarios o días diferentes con {% data variables.product.prodname_enterprise_api %}. Para obtener más información, consulta la sección "[Consola de Administración](/enterprise/{{ currentVersion }}/user/rest/reference/enterprise-admin#enable-or-disable-maintenance-mode)".

## Habilitar o inhabilitar el modo de mantenimientos para todos los nodos de una agrupación

Con la herramienta `ghe-cluster-maintenance`, puedes configurar o anular la configuración del modo de mantenimiento para cada nodo de una agrupación.

```shell
$ ghe-cluster-maintenance -h
# Muestra opciones
$ ghe-cluster-maintenance -q
# Consulta el modo actual
$ ghe-cluster-maintenance -s
# Configura el modo de mantenimiento
$ ghe-cluster-maintenance -u
# Anula la configuración del modo de mantenimiento
```
