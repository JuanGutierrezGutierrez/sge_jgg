# UT05 - Entorno de desarrollo y primer módulo Odoo
## PR0501 - Primer módulo en Odoo

Una vez hemos instalado Odoo, podemos empezar a desarrollar módulos. Para facilitar este desarrollo, Odoo nos proporciona un comando scaffold, el cual nos genera la estructura básica de un módulo.
![](img/foto1.PNG)

El primer fichero que debemos configurar es el manifest. Este contiene los metadatos del módulo, tales como su nombre y descripción. Es muy importante configurar el valor depends, el cual contiene una lista con el nombre de todos los módulos de los cuales depende.

La otra configuración importante en el manifest es el valor data. En este debemos hacer dos cosas: Descomentar el fichero de seguridad, y hacer referencia a todas las vistas que hayamos creado en nuestro proyecto.

## manifest.py

```python
# -*- coding: utf-8 -*-
{
    'name': "lista_salas",

    'summary': """
        Gestor de reserva de salas para una empresa.""",

    'description': """
        Gestor de reserva de salas para una empresa.
    """,

    'author': "Juan Gutierrez",
    'website': "https://www.yourcompany.com",

    # Categories can be used to filter modules in modules listing
    # Check https://github.com/odoo/odoo/blob/16.0/odoo/addons/base/data/ir_module_category_data.xml
    # for the full list
    'category': 'Uncategorized',
    'version': '0.1',

    # any module necessary for this one to work correctly
    'depends': ['base'],

    # always loaded
    'data': [
        'security/ir.model.access.csv',
        'views/views.xml',
        'views/templates.xml',
    ],
    # only loaded in demonstration mode
    'demo': [
        'demo/demo.xml',
    ],
}
```

Debido a que Odoo sigue una estructura ORM, es decir, definimos la información en forma de objetos, aunque después exista una base de datos relacional por debajo, debemos programar estos objetos. Estos reciben el nombre de modelos, y se definen en su propio archivo (por defecto, models.py).

## models.py

```python
# -*- coding: utf-8 -*-

from odoo import models, fields, api


class lista_salas(models.Model):
    _name = 'lista_salas'
    _description = 'lista_salas'

    nombre_sala = fields.Char()
    capacidad = fields.Integer()
    fecha_reserva = fields.Date()
    reservada = fields.Boolean()
    comentarios = fields.Text()
```

A diferencia de otros lenguajes, char no representa un único caracter, sino una cadena de texto pequeña, normalmente de una linea.


Con esto tenemos los objetos que vamos a guardar, en nuestro caso salas de reuniones. Sin embargo, no hemos definido como se van a mostrar por pantalla, ni como va a interactuar el usuario con ellas. Para ello, necesitamos las vistas.

Las vistas tienen su propia carpeta (views), y tenemos que distinguir 3 elementos principales: Los menuItems, las acciones y las views.

Los menuItems definen los botones que va a tener nuestro modulo en la interfaz gráfica de Odoo. Estos pueden tener 3 niveles: El primero está a la altura del resto de modulos, el segundo define las diferentes opciones dentro del módulo, y el tercero ya define las ventanas en concreto que podremos usar, y estarán ligados a una acción.

```python
<!-- Top menu item -->

    <menuitem name="Sistema de gestion de salas" id="lista_salas.menu_root"/>

    <!-- menu categories -->

    <menuitem name="Salas" id="lista_salas.menu_1" parent="lista_salas.menu_root"/>
    <menuitem name="Reservas" id="lista_salas.menu_2" parent="lista_salas.menu_root"/>

    <!-- actions -->

    <menuitem name="Salas disponibles" id="lista_salas.menu_1_list" parent="lista_salas.menu_1"
              action="lista_salas.action_window"/>
    <menuitem name="Salas ocupadas" id="lista_salas.menu_2_list" parent="lista_salas.menu_2"
              action="lista_salas.action_window"/>
```

Si nos fijamos, el principal no tiene valor parent, y los últimos tienen una accion asociada.

Las acciones indican que va a ocurrir cuando pulsemos en ese botón. Hay muchos tipos de acciones en Odoo, por ahora vamos a usar la action window, que muestra la información por pantalla. En concreto, le tendremos que decir a que modelo esta asociada, y como queremos que muestre la información (por defecto, tree/form).

```python
<record model="ir.actions.act_window" id="lista_salas.action_window">
      <field name="name">Lista de salas</field>
      <field name="res_model">lista_salas</field>
      <field name="view_mode">tree,form</field>
</record>
```

Con esto ya le hemos dicho a Odoo que cuando se pulse el boton de salas disponibles, queremos que se nos muestre el modelo de lista de salas. Sin embargo, para eso el modelo necesita tener una vista asociada, que le indique como y que información mostrar.

Para definir la vista, le indicamos a que modelo esta asociada, y que campos del modelo va a mostrar.

```python
<record model="ir.ui.view" id="lista_salas.list">
      <field name="name">Gestion de listas</field>
      <field name="model">lista_salas</field>
      <field name="arch" type="xml">
        <tree>
          <field name="nombre_sala"></field>
          <field name="capacidad"></field>
          <field name="fecha_reserva"></field> 
          <field name="reservada"></field>
          <field name="comentarios"></field> 
        </tree>
      </field>
</record>
```

Con esto, hemos definido el camino completo de dependencias para nuestro módulo. Todo este código se encuentra en el archivo views.xml, al cual hemos referenciado en el manifest. En las siguientes prácticas lo dividiremos en múltiples ficheros para organizarlo mejor.

El módulo ya esta creado, y si lo instalamos en nuestro Odoo compila sin problemas. Sin embargo, no se muestra por pantalla. Esto se debe a que no hemos definido los permisos de seguridad del modelo en el archivo que habiamos descomentado en el manifest.

```python
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_lista_salas_lista_salas,lista_salas.lista_salas,model_lista_salas,base.group_user,1,1,1,1
```

Con esto ya hemos definido la regla de acceso al modelo de la lista de salas, y nuestro módulo funciona correctamente.


# ACTUALIZO CON FOTO CUANDO TENGA ACCESO AL ORDENADOR DEL AULA

---
[Volver a la Unidad 5](../)