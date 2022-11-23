---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.13.8
kernelspec:
  display_name: 'Python 3'
  name: python3
---

(junction_gate)=
# junction_gate
Similar to the junction object, but with the possibility to close of one of the three connected tunnels

|   |   |
|---|---|
|Input connections|<a href="reservoir.html">reservoir</a>, <a href="junction.html">junction</a>, <a href="junction_gate.html">junction_gate</a>|
|Output connections|<a href="plant.html">plant</a>, <a href="junction_gate.html">junction_gate</a>|
|License|SHOP_JUNC_GATE|
|Release version|13.0.0.a|

```{contents}
:local:
:depth: 1
```







## Attributes
```{code-cell} ipython3
:tags: ['remove-input', 'full-width']

import itables as itables
from itables import init_notebook_mode
init_notebook_mode(all_interactive=True, connected=True)
import pandas as pd
from IPython.core.display import HTML

table = pd.read_csv('https://shop.sintef.energy/wp-content/uploads/sites/1/2021/11/attributes_v14.csv')
object_attributes = table[table["Object type"] == "junction_gate"].reset_index().iloc[:, 1:]
for index, row in object_attributes.iterrows():
  object_attributes.at[index, "Attribute name"] = f"""<a href="{row['Object type']}.html#{row['Attribute name'].replace('_', '-')}">{row['Attribute name']}</a>"""
  object_attributes.at[index, "Data type"] = f"""<a href="../datatypes.html#{row['Data type'].replace('_', '-')}">{row['Data type']}</a>"""
itables.show(object_attributes,
  dom='tlip',
  search={'regex': True, "caseInsensitive": True},
  column_filters='header',
  columns=[
    {
      'name': '',
      'className': 'dt-control',
      'orderable': False,
      'data': None,
      'defaultContent': '',
    },
    {
      'name': 'Attribute name',
      'className': 'dt-body-left'
    },
    {
      'name': 'Data type',
      'className': 'dt-body-left'
    },
    {
      'name': 'I/O',
      'className': 'dt-body-left'
    },
    {
      'name': 'License',
      'className': 'dt-body-left'
    },
    {
      'name': 'Version added',
      'className': 'dt-body-left'
    },
    {
      'name': 'Description',
      'visible': False
    }
  ]
)
HTML('''<script>
$('tbody').on('click', 'td.dt-control', function () {
    var tr = $(this).closest('tr');
    var table = $(this).closest('table').DataTable();
    var row = table.row(tr);

    if (row.child.isShown()) {
        // This row is already open - close it
        row.child.hide();
        tr.removeClass('shown');
    } else {
        // Open this row
        row.child("<div align='left'>".concat(row.data()[6], "</div>")).show();
        tr.addClass('shown');
    }
});
</script>''')
```

(junction_gate:add_slack)=
### add_slack
Flag determining whether slack variables should be added to the mass balance of the junction gate (xUnit: NO_UNIT, yUnit: NO_UNIT)


(junction_gate:height_1)=
### height_1
Physical height of the connection point between tunnel_1 and tunnel_2 coming into this junction (xUnit: METER, yUnit: METER)


(junction_gate:loss_factor_1)=
### loss_factor_1
Friction loss factor for tunnel_1 into this junction multiplied with the square of the flow to get head loss (xUnit: S2/M5, yUnit: S2/M5)


(junction_gate:loss_factor_2)=
### loss_factor_2
Friction loss factor for tunnel_2 into this junction multiplied with the square of the flow to get head loss (xUnit: S2/M5, yUnit: S2/M5)


(junction_gate:schedule)=
### schedule
Schedule determining the opening status of this junction gate. -2 means that both tunnel_1 and tunnel_2 are open 0 means that the output from the junction_gate is closed 1 means that only tunnel_1 is connected and 2 means that only tunnel_2 is connected. If two junction_gates are connected in series. The lower junction gate can only have schedule values of 0 or 1 or 2 (xUnit: NO_UNIT, yUnit: NO_UNIT)


(junction_gate:pressure_height)=
### pressure_height
Resulting pressure height from the optimization calculations (xUnit: NO_UNIT, yUnit: METER)


(junction_gate:tunnel_flow_1)=
### tunnel_flow_1
Resulting flow in tunnel_1 (xUnit: NO_UNIT, yUnit: M3/S)


(junction_gate:tunnel_flow_2)=
### tunnel_flow_2
Resulting flow in tunnel_2 (xUnit: NO_UNIT, yUnit: M3/S)


(junction_gate:tunnel_loss_1)=
### tunnel_loss_1
Resulting head loss in tunnel_1 (xUnit: NO_UNIT, yUnit: METER)


(junction_gate:tunnel_loss_2)=
### tunnel_loss_2
Resulting head_loss in tunnel_2 (xUnit: NO_UNIT, yUnit: METER)


