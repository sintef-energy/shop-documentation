# SHOP documentation
The SHOP documentation is built using [Jupyter Book](https://jupyterbook.org/en/stable/intro.html). The command, object and attribute documentation is auto-generated based on the short-descriptions in the SHOP source code. The object description is rendered using [Jinja2](https://jinja.palletsprojects.com/en/3.1.x/) with the template(s) in [](/templates/) by running the script [](/render.py). The whole process is automated with the workflow [](/.github/workflows/jupyter-books.yaml).

For local development and testing, execute the following commands to generate the book:
```
python render.py
jupyter book build .\book\
```
To inspect the book, open `/book/_build/html/intro.html`

## Update documentation
To include additional description, examples and references, please follow this guideline.

### Additional object description
Additional object description can be added by adding markdown files in the `book/doc` folder. The file name should be the same as the object type and should not containt level one headings (#). The content of the file will be automatically merged into the object type documentation by Github Actions.

### Examples
Examples can be added in the [book/examples](book/examples/) folder. Use sub-folders if necessary to keep the structure clean. The examples should be listed in the [table of contents](book/_toc.yml) as a chapter in *Examples and tutorials*. The examples should also be listed under the relevant objects in [](book/objects/cross-references.yaml).

### References
References to scientific publications should be included in [](book/references.bib). Make sure the entry is not a duplicate of existing entries. Add reference to the new publication for the relevant objects in [](book/objects/cross-references.yaml) such that they appear it the automatically generated object documentation.
