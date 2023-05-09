## Sujet
![[Sujet.pdf]]

## Représentation des états
Un état du jeu est représenté par un ensemble de pions blancs, un ensemble de pions noirs, un optional d'un roi blanc (pour ne pas avoir à parcourir tout le tableau pour certaines opérations), un tableau 2 dimensions représentant le jeu dont les cases contiennent les pions (c'est les même pions des ensembles cités précédemment) et les forteresses, et un entier qui représente le compteur du nombre d'actions effectués depuis le début de la partie.

## États initiaux
L'état initial contient 8 pions simples blancs, 16 pions simples noirs, un roi et la taille du tableau dépend du type de jeu (soit Ard-ri, soit Tablut) comme spécifié dans le sujet les pions seront disposés suivant les exemples du sujet et le compteur du nombre d'action est à 0.

## Jeu
Il y a deux joueurs qui jouent tour à tour, les assaillants (pions noirs) et les défendeurs (pions blancs) et chacun fait une seule action à chaque tour
## Actions
Soit $l$ la longueur d'un coté du tableau (7 pour Ard-ri et 9 pour Tablut) les actions possibles sont :
1. declarerForfait (action que j'ai rajouté moi même).
2. deplacer $(x_1, y_1,x_2,y_2)$ si et seulement si :
	- $x_1,y_1,x_2,y_2 \in [0, l-1]$.
	- $(x_1,y_1)$ est la position d'un de nos pions (on ne déplace que ses propres pions).
	- La position $(x_2, y_2)$  du tableau est vide.
	- On a une des conditions suivantes vérifiée :
		1. $x_1 \neq x_2$ et $y_1 = y_2$ et $\nexists p_2$ un pion de position $(x',y_1)$ tel que $x' \in [x_1,x_2]$ (déplacements horizontaux et on ne saute pas au dessus d'un pion).
		2. $y_1 \neq y_2$ et $x_1 = x_2$ et $\nexists p_2$ un pion de position $(x_1,y')$ tel que $y' \in [y_1,y_2]$ (déplacements verticaux et on ne saute pas au dessus d'un pion).
	- Si le pion à déplacer est un pion simple, alors La position $(x_2, y_2)$  du tableau est vide.
	- Si le pion à déplacer est le roi, alors La position $(x_2, y_2)$  du tableau est vide ou est la position d'une forteresse.

## Modèle de transition
Pour ce qui est du modèle de transition :
1. declarerForfait :
	- Si c'est aux assaillant de jouer, déplace le roi dans une forteresse de coin (on le mettra arbitrairement dans le coin haut gauche).
	- Si c'est aux défendeurs de jouer, on enlève le roi du jeu.
1. deplacer $(x_1, y_1,x_2,y_2)$ déplace le pion $p$ de position $(x_1, y_1)$ (l'action doit respecter les règles définis au niveau des actions sinon l'état ne change pas) à la position $(x_2,y_2)$ et enlève du jeu les pions capturés par $p$. Les règles de capture sont les suivantes :
	- Pour la capture de pions simples, si un pion adverse est adjacent à $p$ et que son coté opposé à $p$ est occupé par un de nos pions ou par une forteresse, alors il est capturé
	- Pour la capture du roi blanc, si le roi est adjacent (à 4 pions adverse), ou (à un bord et à 3 pions adverses), ou (à une forteresse et 3 pions adverses) ou  (à une forteresse de coin et 2 pions adverses) ou (que lui et son groupe d'alliés adjacents sont bloqués par les assaillants, en gros qu'aucun membre du groupe ne peut bouger), alors il est capturé

## États finaux
Les états finaux sont les suivants :
- Il n'y a plus de roi blanc (les assaillants gagnent).
- Le roi blanc est dans une forteresse externe (autre que la forteresse centrale) (les défendeurs gagnent).

## Fonction objective
La fonction objective dépend du joueur :
- Pour les assaillants j'ai décidé de le calculer comme suit : nombre de pions noirs $-$ nombre de pions simples blancs $-$ $(2l$ $-$ distance de Manhattan(roi, forteresse de coin la plus proche du roi)).
- Pour les défendeurs j'ai décidé de le calculer comme suit : nombre de pions simples blancs $-$ nombre de pions noirs $+$ ($2l$ $-$ distance de Manhattan(roi, forteresse de coin la plus proche du roi)).

NB : Pion = pion simple ou roi.