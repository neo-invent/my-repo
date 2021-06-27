# Qualité de code et convention de codage

## Introduction

* Qualité de code
<ul>
	<li>Refactoring</li>
	<li>Bonne Pratique</li>
</ul>

* Convention de codage
<ul>
	<li>Règle de style Pythonique Mise en forme du code</li>
</ul>

* Convention de nommage de la BDD
<ul>
	<li>Règle de nommage des éléments de la base de données</li>
</ul>

* Différence entre code fonctionnel et code de qualité
Un code peut être audité pour évaluer sa qualité

* Plusieurs avantages
<ul>
	<li>Facilité de lecture du code Mise à jour plus aisée Moins de bug</li>
	<li>Ensemble de règles de bonne pratique<li>
</ul>

## Utilisation de pylint
Pylint est un logiciel en ligne de commande qui analyse le code en fonction des standards de la PEP 8. Il détecte également les erreurs et le code dupliqué.

* Vérification automatisée de nombreuses règles de style
* Convention de codage (PEP 0008)
* Suggestion d’amélioration du code Interface graphique simple en Tkinter
* Détection d’erreurs (interface, import modules...) Aide au refactoring (code dupliqué...)

### Utiliser Pylint
Commencez par installer Pylint :

```
$ pip install pylint
```

Quand Pylint est installé, lancez l'analyse en tapant: pylint [votrescript.py]

```
$ pylint program.py
```

Puis on obtient le resultat de son analyse.

## Convention de la base de données
### Nomenclature des tables
Pour les tables, on retient pour neoinvent la nommenclature `t_nom_table`. Exemple

```
create table t_pays
(
    pays_id          int,
    pays_code_iso    varchar(10),
    pays_nom         varchar(100),
    pays_nationalite      varchar(100),
    pays_nom_devise	     varchar(50),
    pays_code_iso_dev  varchar(10),
    pays_indicatif_tel    varchar(5),
    creation_date    timestamp, 
    create_by        int ,
    last_update_date timestamp,
    last_update_by   int,	
    constraint       pays_pk primary key (pays_id) using index tablespace neo_indx    
)
```
### Nomenclature des champs
Pour les champs, on precede le nom de la table sans `t_` puis le nom du champ. 
Example:

```
pays_id
pays_code_iso
```
### Nomenclature des sequences
Pour les sequences, on precede le mot `seq` puis le nom de la sequence.

```
drop sequence seq_site;
create sequence seq_site;
```

### Au niveau des requêtes ?
Pour mes requêtes, on les majuscules, distinguant ainsi les parties propres au language des noms des tables et champs.

```SQL
SELECT * FROM t_pays
```
## Convention de nommage du code du projet
### Classes
Toutes les classes sont ecrits en lettres majuscules en debut de mot et devront commencer par C.

```python
class CBlogApp:
    # ...
```

### Méthodes
Les modules sont ecrits en minuscules, tiret du bas et **self** en premier paramètre.

```python
def publish(self):
    #...
```

### Fonctions
Les fonctions sont ecrits en miniscules, tiret du bas.

```python
def post():
    # ...
```
### Variables
Les Variables n'admettent que des lettres miniscules et des tirets du bas.
```python
age = 25
dead = True
price = 12.99
age_list = [17, 18, 19]
```
### Constantes
Les constantes doivent être en majuscule, avec des tirets si nécessaire.

```python
CONSTANT = 'foo'
```
## Commentaires
Les commentaires commencent par un `#` et decrivent une partie specifique du code. Ils permettent de bien comprendre le code.

```python
def __str__(self):
	# permet d'afficher le titre du blog.
        return self.title
```

## Documentation strings (Docstrings) obligatoires
Une doctring est un ensemble de mots qui commence par trois guillemets ouvrants, puis trois guillements fermants.

Chaque fichier python/django doit commencer par un docstring qui contient les informations suivants: son nom, et la date de creation du fichier:

```python
"""
Created on Sun Jun 27
@author: Abdoulaye Diallo
"""
```
Les docstrings doivent aussi servir a documenter un bout de code(classes, methodes, fonctions). Exemple:

```python
class BlogEntry(models.Model):
    """
    Stores a single blog entry, related to :model:`blog.Blog` and
    :model:`auth.User`.

    """
    slug = models.SlugField(help_text="A short label, generally used in URLs.")
    author = models.ForeignKey(User)
    blog = models.ForeignKey(Blog)
    ...

    def publish(self):
        """Makes the blog entry live on the site."""
        ...
```

## Imports
L'import d'une librairie doit être rapide à déceler. Il est également important de bien différencier la source des librairies : standard, externe ou locale. Cela permet de savoir ce qu'il faut installer.

* Les imports sont à placer au début d'un script.
* Ils précèdent les Docstrings.
* Une ligne par librairie. Exemple: 
```python
import os
```

* Une ligne peut néanmoins inclure plusieurs composantes. Exemple:
```python
from django.urls import path, include
```

* L'import doit suivre l'ordre suivant : Bibliothèques standard, Bibliothèques tierces et imports locaux. Sautez une ligne entre chacun de ces blocs.
Par exemple(les commentaires sont a des fins explicatives)
```python
# future
from __future__ import unicode_literals

# standard library
import json
from itertools import chain

# third-party
import bcrypt

# Django
from django.http import Http404
from django.http.response import (
    Http404, HttpResponse, HttpResponseNotAllowed, StreamingHttpResponse,
    cookie,
)

# local Django
from .models import LogEntry

# try/except
try:
    import yaml
except ImportError:
    yaml = None

CONSTANT = 'foo'


class Example:
    # ...
```

## Template Style
Dans le code du modèle Django, placez un (et un seul) espace entre les accolades et le contenu de la balise.
* Faire ceci
```html
{{ foo }}
```

* Au lieu de ceci:
```html
{{foo}}
```

## View Style
* Dans les views Django, le premier paramètre d'une fonction de views doit être appelé **request**.

* Faire ceci:
```python
def my_view(request, foo):
	# ...
```

* Ne pas faire ceci:

```python
def my_view(req, foo):
	# ...
```

## Model style
* Les noms de champs doivent être entièrement en minuscules, en utilisant des traits de soulignement au lieu de camelCase.

* Faire ceci:

```python
class Person(models.Model):
    first_name = models.CharField(max_length=20)
    last_name = models.CharField(max_length=40)
```
