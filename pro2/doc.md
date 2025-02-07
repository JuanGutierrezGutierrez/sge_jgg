# Proyecto de la 2ª Evaluación
## Extensión del modelo de suscripciones mediante métricas y estadísticas

En primer lugar, creamos un nuevo modelo que va a representar la información sobre las estadisticas de las subscripciones.
En este caso vamos a desarrollar el modelo directamente dentro de models.py, por lo que no hace falta enlazar un nuevo archivo.

## models.py

```python
# -*- coding: utf-8 -*-

from odoo import models, fields, api
from datetime import datetime

class subscription(models.Model):
    _name = 'subscription_management.subscription'
    _description = 'subscription_management.subscription'

    name = fields.Char(required=True)
    customer_id = fields.Many2one(required=True, comodel_name='res.partner')
    subscription_code = fields.Char(required=True)
    start_date = fields.Date(required=True)
    end_date = fields.Date()
    duration_months = fields.Integer(compute="_calcular_duracion_meses")
    renewal_date = fields.Date()
    status = fields.Selection(selection=[
        ('active', 'Activa'),
        ('expired', 'Expirada'),
        ('pending', 'Pendiente'),
        ('cancelled', 'Cancelada')
    ])
    is_renewable = fields.Boolean()
    auto_renewal = fields.Boolean()
    price = fields.Float()
    usage_limit = fields.Integer()
    current_usage = fields.Integer()
    use_percent = fields.Float(compute="_calcular_porcentaje_uso")

    @api.depends('start_date', 'end_date')
    def _calcular_duracion_meses(self):
        for subscription in self:
            if subscription.start_date and subscription.end_date:
                subscription.duration_months = abs((subscription.end_date - subscription.start_date).days)/30
            else:
                subscription.duration_months = -1

    @api.depends('usage_limit', 'current_usage')
    def _calcular_porcentaje_uso(self):
        for subscription in self:
            if subscription.usage_limit and subscription.current_usage:
                subscription.use_percent = subscription.current_usage / subscription.usage_limit * 100
            else:
                subscription.use_percent = -1
    
    def retrasar_fecha_fin(self):
        self.end_date = fields.Date.add(self.end_date, days=15)
    

class estadisticas(models.Model):
    _name = 'subscription_management.estadisticas'
    _description = 'subscription_management.estadisticas' 

    fecha_registro = fields.Date()
    suscripciones_activas = fields.Integer()
    ingresos_generados = fields.Float()
    tasa_renovacion = fields.Float(compute="_calcular_tasa_renovacion")
    tasa_cancelacion = fields.Float(compute="_calcular_tasa_cancelacion")
    numero_renovaciones = fields.Integer()
    nuevas_suscripciones = fields.Integer()
    suscripciones_canceladas = fields.Integer()
    clientes_nuevos = fields.Integer()
    clientes_recurrentes = fields.Integer()
    ingreso_promedio_por_usuario = fields.Float(compute="_calcular_ingreso_promedio")
    tasa_conversion = fields.Float()
    tasa_perdida_clientes = fields.Float()
    valor_total_generado = fields.Float()
    costo_adquisicion_clientes = fields.Float()
    notas = fields.Text()
    suscripcion = fields.One2many(
        comodel_name = 'subscription_management.subscription',
        inverse_name = 'subscription_code'
    )

    @api.depends('numero_renovaciones', 'suscripciones_activas')
    def _calcular_tasa_renovacion(self):
        for estadistica in self:
            if estadistica.suscripciones_activas <= 0:
                estadistica.tasa_renovacion = 0
            else:
                estadistica.tasa_renovacion = (estadistica.numero_renovaciones / estadistica.suscripciones_activas)*100 

    @api.depends('suscripciones_canceladas', 'suscripciones_activas')
    def _calcular_tasa_cancelacion(self):
        for estadistica in self:
            if estadistica.suscripciones_activas <= 0:
                estadistica.tasa_cancelacion = 0
            else:
                estadistica.tasa_cancelacion = (estadistica.suscripciones_canceladas / estadistica.suscripciones_activas)*100

    @api.depends('valor_total_generado', 'suscripciones_activas')
    def _calcular_ingreso_promedio(self):
        for estadistica in self:
            if estadistica.suscripciones_activas <= 0:
                estadistica.ingreso_promedio_por_usuario = 0
            else:
                estadistica.ingreso_promedio_por_usuario = (estadistica.valor_total_generado / estadistica.suscripciones_activas)*100
```


Para que este modelo sea visible, hay que otorgarle la regla de seguridad. Como es un nuevo modelo, necesitamos añadir una nueva regla a las ya existentes.

## archivo seguridad

```python
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_subscription_management_subscription_management,subscription_management.subscription_management,model_subscription_management_subscription,base.group_user,1,1,1,1
access_subscription_management_estadisticas,subscription_management.estadisticas,model_subscription_management_estadisticas,base.group_user,1,1,1,1
```


Ahora solo nos queda crear una seccion en el menu a partir de la cual acceder a la vista, y crear la propia vista en si.
Primero, definimos tanto el menu, como la accion que va a tener asociada. Estos se encuentran en views.xml, que ya teniamos definido.


## views.xml 

```xml
<menuitem name="Estadisticas" id="subscription_management.menu_2" parent="subscription_management.menu_root"/>

<menuitem name="Estadisticas" id="subscription_management.menu_estadisticas" parent="subscription_management.menu_2"
              action="subscription_management.accion_vista_estadisticas"/>      

<record model="ir.actions.act_window" id="subscription_management.accion_vista_estadisticas">
      <field name="name">subscription_management estadisticas</field>
      <field name="res_model">subscription_management.estadisticas</field>
      <field name="view_mode">tree,form</field>
      <field name="view_id" ref="subscription_management.vista_estadisticas"/>
    </record>
```


Sin embargo, si que vamos a crear un nuevo archivo para definir la vista. Por lo tanto, despues lo tendremos que enlazar en el manifest.

## vista_estadistica.xml

```xml
<odoo>
    <data>
        <record model="ir.ui.view" id="subscription_management.vista_estadisticas">
        <field name="name">subscription_management estadisticas</field>
        <field name="model">subscription_management.estadisticas</field>
        <field name="arch" type="xml">
            <form>
                <sheet>
                    <group>
                        <field name="fecha_registro" string="Fecha del registro"/>
                        <field name="notas" string="Notas"/>
                        <field name="suscripcion" string="Subscripcion"/>
                    </group>

                    <notebook>
                    <page string="Campos simples">
                        <group>
                            <field name="suscripciones_activas" string="Subscripciones activas"/>
                            <field name="ingresos_generados" string="Ingresos generados"/>
                            <field name="numero_renovaciones" string="Numero de renovaciones"/>
                            <field name="nuevas_suscripciones" string="Nuevas subscripciones"/>
                            <field name="suscripciones_canceladas" string="Subscripciones canceladas"/>
                            <field name="clientes_nuevos" string="Clientes nuevos"/>
                            <field name="clientes_recurrentes" string="Clientes recurrentes"/>
                            <field name="valor_total_generado" string="Valor total generado"/>
                            <field name="costo_adquisicion_clientes" string="Coste de adquisicion"/>
                        </group>
                    </page>

                    <page string="Campos calculados y tasas">
                        <group>
                            <field name="tasa_renovacion" string="Tasa de renovacion"/>
                            <field name="tasa_cancelacion" string="Tasa de cancelacion"/>
                            <field name="ingreso_promedio_por_usuario" string="Ingreso promedio"/>
                            <field name="tasa_conversion" string="Tasa de conversion"/>
                            <field name="tasa_perdida_clientes" string="Tasa de perdida"/>
                        </group>
                    </page>
                    </notebook>
                </sheet>
            </form>
        </field>
        </record>
    </data>
</odoo>
```


## manifest

```xml
# -*- coding: utf-8 -*-
{
    'name': "subscription_management",

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
        'views/templates.xml',
        'views/static_web.xml',
        'views/vista_web_dinamica.xml',
        'views/vista_formulario.xml',
        'views/vista_estadistica.xml',
        'views/views.xml'
    ],
    # only loaded in demonstration mode
    'demo': [
        'demo/demo.xml',
    ],
}
```

Con esto, hemos definido el nuevo modelo, y el menu, la accion y la vista necesarias para poder trabajar con el.

No puedo adjuntar capturas del funcionamiento, ya que no tengo acceso al ordenador del aula donde se encuentra instalado mi odoo.


---
[Volver al indice](../)