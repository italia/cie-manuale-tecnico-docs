# Regole Tecniche CIE eID SAML

[![Get invited](https://slack.developers.italia.it/badge.svg)](https://slack.developers.italia.it/)
[![Docs Italia](https://docs.italia.it/media/static/projects/badges/passing.svg)](https://docs.italia.it/italia/cie-manuale-tecnico-docs/it/master/index.html)
[![Documentation](https://img.shields.io/badge/Documentation-Docs%20Italia-blue.svg)](https://docs.italia.it/italia/cie-manuale-tecnico-docs/)

---

## Table of Contents

- [Introduction](#introduction)
- [Preview](#preview)
- [Build](#build)
- [Contributing](#how-to-contribute)
- [License](#license)

## Introduction

This repository hosts the sphinx project tree of CIE eID Technical Manual for the authentication scheme "Entra con CIE". 
This repository is structured to be compliant with 
[Docs Italia](https://docs.italia.it/italia/developers-italia/publiccodeyml/it/master/index.html).
This is why the content of the relevant folders will be compiled and rendered inside such platform.
`Docs Italia` is designed to support documents, localized in different languages and for this
reason it is the reference platform for displaying this standard.


## Preview

The current version is published at this [link](https://italia.github.io/cie-manuale-tecnico-docs/master/it/).

### Preview of a branch

Preview of other branches can be navigated by adding the branch name in the webpath, as follows:

 - https://italia.github.io/cie-manuale-tecnico-docs/<branch_name>/it/


## Build

HTML
````
pip install -r requirements.txt

sphinx-build -b html -d html/it/doctrees docs/it/  html/it

````

ODT
````
sudo apt install pandoc
sphinx-build -b singlehtml docs/it/  html/
cd html
pandoc -o cie-eid-saml-docs.odt index.html
````

## How to contribute

Please help development by reporting problems, suggesting changes and asking questions. 

Feel free to open Pull Requests and to present a problem with an Issue.

If you never used GitHub, get up to speed with [Understanding the GitHub Flow](https://guides.github.com/introduction/flow/) or follow one of the great free interactive courses in the [GitHub learning lab](https://lab.github.com/) on working with GitHub and working with MarkDown, the syntax this project's documentation is in.

This project is [licenced CC-0](LICENSE), which essentially means that the project, along with your contributions is in the Public Domain in whatever jusrisdiction possible, and everyone can do whatever they want with it.

## License

The project is covered by a [CC-0](LICENSE) license.
