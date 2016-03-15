Création d'un package installable pour Python avec setup.py (setuptools)
=======================================================================

Tuto inspiré de [Sam et Max](http://sametmax.com/creer-un-setup-py-et-mettre-sa-bibliotheque-python-en-ligne-sur-pypi/)
Il ne s'agit pas d'une copie, mais plus d'un pense-bête

Arborescence et fichiers nécessaires
---------------------------------------

    nom_du_package
    |-- MANIFEST.in
    |-- README.md
    |-- setup.py
    |-- nom_du_package
        |-- __init__.py
        |-- fichiers_de_la_librairie.py...

Rendre le package user friendly
------------------------------

- Rajouter dans le `__init__.py` les imports nécessaires pour simplifier l'utilisation de la librairie et la version, exemple:


    __version__ = "0.0.1"
    from core import class

- Simplifier l'autocompletition : limiter ce qui est importable avec `__all__` dans les différents fichier


    __all__ = ['fonction1', 'fonction2']

- Commenter (en anglais) correctement le code avec les docstrings qui vont bien
  - *docstring* dans `__init__.py` pour décrire le package
  - *docstring* au sommet des fichiers `.py` pour décrire les modules
  - *docstring* dans les fonctions pour les décrire
  - Rajouter des tests unitaires et exemples si besoins


README.md
---------

Mettre dans ce fichier toutes les informations utiles à l'utilisateur du package:
- A quoi sert la lib
- Comment l'installer
- Un exemple concret d'utilisation
- Un lien vers la doc si elle existe
- Format Markdown.

Ce fichier va ressembler à un mélange des docstrings mais en amélioré.

MANIFEST.in
-----------

Ce fichier va contenir une liste de tous les fichiers non Python qu’on veut inclure dans le package.

    include *.md *.rst
    recursive-include dir1 *.txt *.rst
    recursive-include dir2 *.png *.jpg *.gif
    recursive-include dir2 *.css *.js *.coffee

Setup.py
--------

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
     
    from setuptools import setup, find_packages
     
    # notez qu'on import la lib
    # donc assurez-vous que l'importe n'a pas d'effet de bord
    import sm_lib
     
    # Ceci n'est qu'un appel de fonction. Mais il est trèèèèèèèèèèès long
    # et il comporte beaucoup de paramètres
    setup(
     
        # le nom de votre bibliothèque, tel qu'il apparaitre sur pypi
        name='sm_lib',
     
        # la version du code
        version=sm_lib.__version__,
     
        # Liste les packages à insérer dans la distribution
        # plutôt que de le faire à la main, on utilise la foncton
        # find_packages() de setuptools qui va cherche tous les packages
        # python recursivement dans le dossier courant.
        # C'est pour cette raison que l'on a tout mis dans un seul dossier:
        # on peut ainsi utiliser cette fonction facilement
        packages=find_packages(),
     
        # votre pti nom
        author="Sam et Max",
     
        # Votre email, sachant qu'il sera publique visible, avec tous les risques
        # que ça implique.
        author_email="lesametlemax@gmail.com",
     
        # Une description courte
        description="Proclame la bonne parole de sieurs Sam et Max",
     
        # Une description longue, sera affichée pour présenter la lib
        # Généralement on dump le README ici
        long_description=open('README.md').read(),
     
        # Vous pouvez rajouter une liste de dépendances pour votre lib
        # et même préciser une version. A l'installation, Python essayera de
        # les télécharger et les installer.
        #
        # Ex: ["gunicorn", "docutils >= 0.3", "lxml==0.5a7"]
        #
        # Dans notre cas on en a pas besoin, donc je le commente, mais je le
        # laisse pour que vous sachiez que ça existe car c'est très utile.
        # install_requires= ,
     
        # Active la prise en compte du fichier MANIFEST.in
        include_package_data=True,
     
        # Une url qui pointe vers la page officielle de votre lib
        url='http://github.com/sametmax/sm_lib',
     
        # Il est d'usage de mettre quelques metadata à propos de sa lib
        # Pour que les robots puissent facilement la classer.
        # La liste des marqueurs autorisées est longue:
        # https://pypi.python.org/pypi?%3Aaction=list_classifiers.
        #
        # Il n'y a pas vraiment de règle pour le contenu. Chacun fait un peu
        # comme il le sent. Il y en a qui ne mettent rien.
        classifiers=[
            "Programming Language :: Python",
            "Development Status :: 1 - Planning",
            "License :: OSI Approved",
            "Natural Language :: French",
            "Operating System :: OS Independent",
            "Programming Language :: Python :: 2.7",
            "Topic :: Communications",
        ],
     
     
        # C'est un système de plugin, mais on s'en sert presque exclusivement
        # Pour créer des commandes, comme "django-admin".
        # Par exemple, si on veut créer la fabuleuse commande "proclame-sm", on
        # va faire pointer ce nom vers la fonction proclamer(). La commande sera
        # créé automatiquement. 
        # La syntaxe est "nom-de-commande-a-creer = package.module:fonction".
        entry_points = {
            'console_scripts': [
                'proclame-sm = sm_lib.core:proclamer',
            ],
        },
     
        # A fournir uniquement si votre licence n'est pas listée dans "classifiers"
        # ce qui est notre cas
        license="WTFPL",
     
        # Il y a encore une chiée de paramètres possibles, mais avec ça vous
        # couvrez 90% des besoins
     )

Tester si tout marche bien
--------------------------

    python setup.py install

Publier sur Pypi
----------------

    python setup.py register

Si ça répond :

    Registering sm_lib to http://pypi.python.org/pypi
    Server response (200): OK

C'est que le nom est dispo.

Pour créer la distribution source :

    python setup.py sdist upload

Fini. Le package est installable par `pip`

**Penser à modifier `__version__` pour un nouvel upload.**