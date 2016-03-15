Construire son environement de travail en Python
================================================

Python 3.5.1
------------

### Installation

On met python dans /opt pour garder un système propre

    cd /tmp

Prendre la dernière version de Python :

    sudo apt-get install build-essential libncurses5-dev libncursesw5-dev libreadline6-dev libdb-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev
    wget https://www.python.org/ftp/python/3.5.1/Python-3.5.1.tar.xz
    tar -Jxvf Python-3.5.1.tar.xz
    cd Python-3.5.1
    ./configure --prefix=/opt/python-3.5.1
    make
    sudo make install

### Virtualenv

    cd 'projetPath'
    /opt/python-3.5.1/bin/pyvenv virtualenv-3.5.1

Pour rentrer dans l'environnement

    source virtualenv-3.5.1/bin/activate

Pour quitter l'environnement :

    deactivate

### Utilisation de pip

Vérifier qu'on utilise le bon pip :

    which pip

Vérifier que l'environnement est vierge :

    pip freeze

Installer les dépendances

    pip install ...

Voir les dépendances

    pip freeze

Sauvegarder les dépendances

    pip freeze > requirements.txt

Installer les dépendances

    pip install -r requirements.txt