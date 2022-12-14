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

(command-table)=
# Commands

```{code-cell} ipython3
:tags: ['remove-input', 'full-width']

import itables as itables
from itables import init_notebook_mode
import pandas as pd
from IPython.core.display import HTML

init_notebook_mode(all_interactive=True, connected=True)
table = pd.read_csv('https://shop.sintef.energy/wp-content/uploads/sites/1/2021/11/commands_v14.csv').reset_index()
for index, row in table.iterrows():
  table.at[index, "Command"] = f"""<a href="commands/{row["Command"].replace(' ', '_')}.html">{row["Command"]}</a>"""
itables.show(
  table,
  dom='tlip',
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
      'name': 'Command',
      'className': 'dt-body-left'
    },
    {
      'name': 'Options',
      'className': 'dt-body-left'
    },
    {
      'name': 'Value',
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
