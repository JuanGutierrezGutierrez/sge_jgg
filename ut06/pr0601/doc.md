# UT06 - Desarrollo de módulos de Odoo: Modelo y vista
## PR0601 - Campos del modelo

Para esta práctica, vamos a seguir el mismo procedimiento que en todas las anteriores. Generamos la estructura principal mediante el comando scaffold, y definimos el manifest.

```xml
# -*- coding: utf-8 -*-
{
    'name': "gestion_productos",

    'summary': """
        Short (1 phrase/line) summary of the module's purpose, used as
        subtitle on modules listing or apps.openerp.com""",

    'description': """
        Long description of module's purpose
    """,

    'author': "My Company",
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

Como tenemos un único modelo, podemos meter toda la información del menu y las vistas en un único fichero por simplicidad.

```xml
<odoo>
  <data>
    <!-- explicit list view definition -->

    <record model="ir.ui.view" id="gestion_productos.list">
      <field name="name">gestion_productos list</field>
      <field name="model">gestion_productos.gestion_productos</field>
      <field name="arch" type="xml">
        <tree>
          <field name="nombre"/>
          <field name="descripcion"/>
          <field name="codigo"/>
          <field name="imagen"/>
          <field name="categoria"/>
          <field name="tipo_producto"/>
          <field name="precio_venta"/>
          <field name="stock_disponible"/>
          <field name="fecha_creacion"/>
          <field name="fecha_ultima_actualizacion"/>
          <field name="activo"/>
          <field name="peso_producto"/>
        </tree>
      </field>
    </record>


    <!-- actions opening views on models -->

    <record model="ir.actions.act_window" id="gestion_productos.action_window">
      <field name="name">Lista de productos</field>
      <field name="res_model">gestion_productos.gestion_productos</field>
      <field name="view_mode">tree,form</field>
    </record>

    <!-- server action to the one above -->
<!--
    <record model="ir.actions.server" id="gestion_productos.action_server">
      <field name="name">gestion_productos server</field>
      <field name="model_id" ref="model_gestion_productos_gestion_productos"/>
      <field name="state">code</field>
      <field name="code">
        action = {
          "type": "ir.actions.act_window",
          "view_mode": "tree,form",
          "res_model": model._name,
        }
      </field>
    </record>
-->

    <!-- Top menu item -->

    <menuitem name="Gestion de productos" id="gestion_productos.menu_root"/>

    <!-- menu categories -->

    <menuitem name="Productos" id="gestion_productos.menu_1" parent="gestion_productos.menu_root"/>

    <!-- actions -->

    <menuitem name="Lista" id="gestion_productos.menu_1_list" parent="gestion_productos.menu_1"
              action="gestion_productos.action_window"/>

  </data>
</odoo>
```

Y como solo tenemos un modelo, solo necesitamos una regla de seguridad.

```python
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_gestion_productos_gestion_productos,gestion_productos.gestion_productos,model_gestion_productos_gestion_productos,base.group_user,1,1,1,1
```

Ahora, lo interesante de esta práctica que no habiamos visto hasta este momento, son los diferentes atributos que pueden tener los campos de un modelo en Odoo. Primero, vemos la definición completa del modelo.

```python
# -*- coding: utf-8 -*-

from odoo import models, fields, api


class gestion_productos(models.Model):
    _name = 'gestion_productos.gestion_productos'
    _description = 'gestion_productos.gestion_productos'

    nombre = fields.Char()
    descripcion = fields.Text()
    codigo = fields.Integer(required=True)
    imagen = fields.Image()
    categoria = fields.Selection(
        selection=[
            ('jardin', 'Jardín'),
            ('hogar', 'Hogar'),
            ('electrodomesticos', 'Electrodomésticos')
        ]
    )
    tipo_producto = fields.Boolean(string = 'Tipo')
    currency_id = fields.Many2one('res.currency', string='MonedaDefecto', required=True)
    precio_venta = fields.Monetary(currency_field='currency_id')
    stock_disponible = fields.Integer(string = 'Cantidad Stock')
    fecha_creacion = fields.Date(default = fields.Date.today, string = 'Fecha de creacion')
    fecha_ultima_actualizacion = fields.Date(default = fields.Date.today, string = 'Fecha de ultima actualizacion')
    activo = fields.Boolean(default=True)
    peso_producto = fields.Float(
        string = 'Peso',
        digits=(7,2)
    )
    
```

El atributo required del campo código es un boolean, que por defecto tiene valor false. De ponerlo a true, como en este caso, obligará a rellenar este campo para poder crear un objeto de este modelo. El atributo required es general, todos los tipos de campos pueden tenerlo.

El atributo selection del campo categoria es un atributo especial de los campos de tipo selection. Este recibe una lista de tuplas que representan todas las opciones seleccionables. El primer valor de la tupla es el valor interno que tenemos en el código de cada opción, y el segundo es lo que se muestra por pantalla.

El atributo string del campo tipo_producto es atributo general de todos los campos, en el cual indicamos que valor queremos que se muestre por pantalla. Por defecto, se muestra el mismo nombre que tenga el atributo en el código.

El atributo currency_field del campo precio_venta es un atributo especial de los campos monetary, y hace referencia a que moneda vamos a utilizar para definir esa cantidad monetaria. Por lo tanto, suele tener una referencia a un campo currency_id, el cual se encuentra relacionado con la tabla res.currency, donde se almacenan las monedas disponibles. Así, podremos elegir una moneda en currency_id, que será la que utilice precio_venta.

El atributo default de fecha_creacion es un atributo general de todos los campos, en el cual indicamos un valor por defecto que tendra el campo cuando intentemos crear un objeto de un modelo. En este caso, hacemos uso de la funcion today del campo date, la cual nos devuelve la fecha actual en el momento de ejecutar el código.

El atributo digits de peso_producto es un atributo especial de los campos float, el cual recibe una tupla. El primer digito de la tupla es la longitud total del número, y el segundo digito indica cuantos digitos de esa longitud total van a ser decimales. En este caso, el peso puede llegar a tener 7 digitos, 2 de los cuales serán decimales.


Con esto, ya ha quedado definido el módulo, y podemos generar objetos de este modelo con éxito.

# ACTUALIZO CON FOTO CUANDO TENGA ACCESO AL ORDENADOR DEL AULA


---
[Volver a la Unidad 6](../)