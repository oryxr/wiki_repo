LTSpice IV
==========

Modèles de composants
---------------------
Interrupteur :

    .model SW SW(Ron=1 Roff=1Meg Vt=.5 Vh=-.4)

Gestion des paramètres
----------------------
Pour pouvoir régler un paramètre comme on le souhaite, il faut le mettre entre `{...}` et utiliser la directive : `.param ...=valeur` (p.281)
On peut initialiser des paramètres dans le sous circuit et si on souhaite les modifier dans le circuit principale, utiliser modifier les paramètres dans la partie param (clic droit sur le sous circuit)

Pour pouvoir faire varier ce paramètre et simuler à chaque étape, utiliser la directive : `.step param ... min max pas` (p. 284)
