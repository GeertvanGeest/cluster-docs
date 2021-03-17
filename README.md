# IBU cluster wiki

This website is hosted at: [https://doc.bioinformatics.unibe.ch/cluster_wiki/](https://doc.bioinformatics.unibe.ch/cluster_wiki/). 
Small edits can be directly commited to master. Larger edits (i.e. e.g. adding/removing pages) would require a merge request. 

To sync your changes to the hosted version, follow the instructions at [https://doc.bioinformatics.unibe.ch/apps-doc/documentation/usage/](https://doc.bioinformatics.unibe.ch/apps-doc/documentation/usage/). 

## Hosting it locally

This wiki is generated with [MkDocs](https://www.mkdocs.org/), with the theme [Material](https://squidfunk.github.io/mkdocs-material/).

To host it locally, install MkDocs:
```bash
pip install mkdocs
```

and Material:
```bash
pip install mkdocs-material
```

Host it with:
```bash
mkdocs serve
```

Check it out with your browser at [http://localhost:8000/](http://localhost:8000/)
