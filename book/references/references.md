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

# List of publications
This overview shows papers published by SINTEF that are relevant for SHOP.

```{code-cell} ipython3
:tags: ['remove-input', 'full-width']

import itables as itables
from itables import init_notebook_mode
init_notebook_mode(all_interactive=True, connected=True)
import pandas as pd
from IPython.core.display import HTML
import bibtexparser
import yaml

with open('references.bib', encoding='utf8') as bibfile:
  bib_list = bibtexparser.load(bibfile)
table = pd.DataFrame(bib_list.entries)
## Available entries
# ['year', 'url', 'volume', 'title', 'pages', 'keywords', 'journal',
#      'isbn', 'doi', 'author', 'ENTRYTYPE', 'ID', 'abstract', 'publisher',
#      'month', 'issn', 'issue'],
table = table[['author', 'title', 'journal', 'year', 'url', 'doi', 'abstract']].reset_index()
with open('author-links.yml', encoding='utf8') as f:
  author_links = yaml.load(f, Loader=yaml.FullLoader)
for index, row in table.iterrows():
  if not pd.isnull(row["url"]):
    table.at[index, "url"] = f'<a href="{row["url"]}">Url</a>'
  else:
    table.at[index, "url"] = ""
  if not pd.isnull(row["doi"]):
    table.at[index, "doi"] = f'<a href="https://doi.org/{row["doi"]}">Doi</a>'
  else:
    table.at[index, "doi"] = ""
  author_list = [r.strip() for  r in row["author"].split(" and ")]
  author_string = ""
  for i,r in enumerate(author_list):
    if r in author_links:
      a = f"""<a href="{author_links[r]}">{r}</a>"""
    else:
      a = r
    if i == len(author_list) - 1:
      author_string += f", and {a}"
    elif i > 0:
      author_string += f", {a}"
    else:
      author_string += a
  table.at[index, "author"] = author_string
itables.show(table,
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
      'name': 'author',
      'className': 'dt-body-left'
    },
    {
      'name': 'title',
      'className': 'dt-body-left'
    },
    {
      'name': 'journal',
      'className': 'dt-body-left'
    },
    {
      'name': 'year',
      'className': 'dt-body-left'
    },
    {
      'name': 'url',
      'className': 'dt-body-left'
    },
    {
      'name': 'doi',
      'className': 'dt-body-left'
    },
    {
      'name': 'abstract',
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
        row.child("<div align='left'>".concat(row.data()[7], "</div>")).show();
        tr.addClass('shown');
    }
});
</script>''')

```

