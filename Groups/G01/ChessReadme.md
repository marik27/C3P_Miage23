## Kata  Implements 9 queens problems

> Dans mon kata, il était question de résoudre le problème des 9 reines dans le jeu d'echecs, 
Pourquoi le Backtracking pour le problème des 9 reines ? 
Le problème des 9 reines consiste à placer 9 reines sur un échiquier 9×9 de manière à ce qu'aucune ne puisse en attaquer une autre, c'est-à-dire :

    Pas deux reines sur la même ligne.
    Pas deux reines sur la même colonne.
    Pas deux reines sur la même diagonale.

Le backtracking est un choix naturel pour ce problème parce que :

    Exploration Arborescente : Le backtracking explore toutes les configurations possibles de manière systématique.
    Élagage des branches non valides : Dès qu'une configuration ne respecte pas les règles, elle est abandonnée immédiatement.
    Solution Complète : Il garantit de trouver toutes les solutions possibles si elles existent.

2. Principe de l'Algorithme Backtracking

L'algorithme de Backtracking est une approche récursive qui consiste à :

    Placer une reine dans une ligne.
    Vérifier si le placement est sûr (aucune attaque).
    Si le placement est valide : Passer à la ligne suivante et répéter.
    Si le placement échoue : Revenir en arrière (Backtrack) à la ligne précédente et essayer la colonne suivante.
 Étapes du Backtracking :

    Ligne par Ligne : On place les reines une par une, ligne après ligne.
    Vérification des Conflits : Chaque placement vérifie les lignes, colonnes et diagonales.
    Récursion : Une fois qu'une reine est placée correctement, on passe à la ligne suivante.
    Retour Arrière : Si un placement ne permet pas de placer toutes les reines, on revient au placement précédent et on essaie une autre colonne.
3. Implémentation Étape par Étape
3.1 Fonction Principale : solveNQueens:

solveNQueens: size
"Résout le problème des N reines pour un échiquier de taille donnée."

    | board solutions |
    board := Array new: size.
    solutions := OrderedCollection new.

    "Démarre le placement depuis la première ligne"
    self placeQueenOnRow: 1 board: board size: size solutions: solutions.
    
    "Afficher la première solution trouvée"
    solutions isEmpty ifFalse: [
        self displaySolution: solutions first.
        Transcript show: 'Solution trouvée et affichée.'; cr.
    ] ifTrue: [
        Transcript show: 'Aucune solution trouvée.'; cr.
    ].

Explication :

    On initialise un tableau board pour suivre la position des reines.
    On utilise une liste solutions pour stocker les solutions valides.
    La méthode placeQueenOnRow: est appelée pour commencer à placer les reines à partir de la première ligne.

3.2 Fonction de Placement Récursif : placeQueenOnRow:

placeQueenOnRow: row board: board size: size solutions: solutions
"Place une reine sur une ligne spécifique et passe à la suivante."

    row > size ifTrue: [
        "Toutes les reines sont placées, ajouter une solution valide"
        solutions add: board copy.
        ^self.
    ].

    1 to: size do: [:col |
        (self isSafeAtRow: row column: col board: board) ifTrue: [
            board at: row put: col.
            self placeQueenOnRow: row + 1 board: board size: size solutions: solutions.
            board at: row put: nil.
        ]
    ].

Explication :

    Pour chaque colonne dans la ligne actuelle (1 to: size), on vérifie si le placement est sûr (isSafeAtRow:).
    Si oui, on place la reine et on passe à la ligne suivante.
    Si aucune colonne n'est valide, on revient à la ligne précédente et on essaie une autre colonne.

3.3 Fonction de Validation : isSafeAtRow:column:

isSafeAtRow: row column: col board: board
"Vérifie si une reine peut être placée en toute sécurité."

    1 to: row - 1 do: [:r |
        | c |
        c := board at: r.
        c ifNotNil: [
            (c = col or: [(c - col) abs = (r - row) abs]) ifTrue: [^false].
        ].
    ].
    ^true

Explication :

    On vérifie chaque reine déjà placée.
    On vérifie :
        Même colonne (c = col)
        Même diagonale ((c - col) abs = (r - row) abs)
    Si une condition est remplie, le placement n'est pas sûr.
Pourquoi le Backtracking est-il Efficace Ici ?

Exploration Complète : Toutes les configurations possibles sont explorées de manière exhaustive.
Élagage Intelligent : Les configurations invalides sont détectées et abandonnées très tôt, ce qui économise du temps et des ressources.
Résultat Garanti : S'il existe une solution, elle sera trouvée.

Lancement du jeu avec les 9 reines, dans la méthode `initialize` j'ai rajouté 
`self initializeWithSize: 9.
self solveNQueens: 9.`
pour lancer le tableau avec N=8 on a juste à changer 9 par 8.