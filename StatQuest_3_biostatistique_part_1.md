(Bio)statistics in R
================

Introduction
============

L'objectif de ce projet est de fournir les aspects fondamentaux des biostatistiques à l'aide du logiciel R. Pour cela le [cours](https://www.kaggle.com/ruslankl/bio-statistics-in-r-part-1) de Ruslan Klymentiev servira de support principal et il sera soupoudré de [l'aide mémoire](https://cran.r-project.org/doc/contrib/Herve-Aide-memoire-statistique.pdf) (en français) de Maxime Hervé. De plus il envisage de comprendre les formulations mathématique en essayant d'expliquer les formules mathématique et de les illustrer avec des exemples.

Table of contents
-----------------

-   [Setting up the environment](#Setting-up-the-environment)
-   [Penser de manière probabiliste](#Penser-de-manière-probabiliste)
-   [Définitions](#définitions)
-   [Notation mathématique](#notation-mathématique)
    -   [La quantification](#la-quantification)

Setting up the environment
==========================

Comme il existe différente façon de pouvoir générer de l'aléatoire, en utilisant la fonction set.seed() on permet de fixer l'aléatoire et d'avoir un code reproductible d'une machine à l'autre.

``` r
ipak(packages)
```

    ##      ggplot2         plyr       gtools        dplyr     ggridges 
    ##         TRUE         TRUE         TRUE         TRUE         TRUE 
    ##      stringr RColorBrewer      lattice       scales      plotrix 
    ##         TRUE         TRUE         TRUE         TRUE         TRUE 
    ##    gridExtra     reshape2        tidyr randomForest      cowplot 
    ##         TRUE         TRUE         TRUE         TRUE         TRUE 
    ##         repr      Metrics          AUC 
    ##         TRUE         TRUE         TRUE

``` r
set.seed(123)
```

Penser de manière probabiliste
==============================

Dans son post Ruslan Klymentiev introduit la pensé probabiliste de la manière suivante :

-   Penser de manière probabiliste est essentiel pour essayer d'estimer, en utilisant des outils logique et mathématique, la vraissemblence de toute sorte de résultat à venir. C'est l'un des meilleur moyen pour améliorer la prise de décison. Dans un monde où chaque moment est déterminé par un ensemble de facteurs très complexes, penser de manière probabiliste permet d'identifier le résultat **qui a le plus de chance d'arriver**. Lorsque l'ont connait cela, les décisions peuvent être plus précise et plus effective. Source [Farnam Street](https://fs.blog/2018/05/probabilistic-thinking/)

-   Par exemple, lorsque le problème semble insoluble, mais que nous commençons à penser de manière probabiliste, nous pourrions résoudre le problème avec des techniques de monte carlo.

Définitions
-----------

Pour chaque définitions, le mots définit est en gras, sa traduction anglaise est entre parenthèse et sa notation mathématique est indiqué par `,noté ...,` et à la fin on peut avoir la même chose dite en math.

-   **L'univers** (sample space), noté *Ω* (parfois *U* ou *S*), représente l'**ensemble** de tous les résultats d'une experience aléatoire qui peuvent être obtenue.

-   Un **événement** (event), noté *E*, lié à une expérience aléatoire est un **ensemble** dont les éléments sont des résultats possible pour cette expérience. *E* est donc un sous ensemble de *Ω*.

-   **Evénement élémentaire** (Elementary/sample event), noté *ω*, est un résultat spécifique d'une expérience.

-   L'ensemble vide (null event), noté ∅, ne contient aucun élément (d'ou le faite qu'il soit vide).

-   L'évenement élémentaire ce réalise si et seumlement si l'ont obtient le résultat d'un évenement. *ω* ∈ *E* : *E* ce réalise quand *ω* ce réalise.

-   L'évenement élémentaire ne ce réalise pas si et seumlement si l'ont obtient pas le résultat d'un évenement. $E :E $ ce réalise pas quand *ω* ce réalise.

-   L'apparition de *E* implique l'apparition de *F* et donc *E* est un **sous ensemble** de *F*. *E* ⊂ *F* .

-   L'événement est présent dans *E* et aussi dans *F* (on parle d'intersection) | *E* ∩ *F* | **Fonction R** `E & F` .

-   Les évenements s'exclue mutuellement si les deux évenements ne peuvent pas avoir lieux en même temps | *E* ∩ *F* = ∅ .

-   L'apparition de au moins un élément de *E* ou de *F* se réalise (union) | *E* ∪ *F* | **Fonction R** `E | F` .

-   *E*<sup>*C*</sup> ou $\\bar{E}$

Notation mathématique
---------------------

### La quantification

-   **Pour tout** *x* dans *A*, la propriété *P*(*x*) est vrai | (∀*x* ∈ *A*)*P*(*x*)

-   ∃

-   ∃!

-   ∃?

-   ⟹

Remarque : Si jamais on cherche comment écrir les symboles, regarder ce [site](http://csrgxtu.github.io/2015/03/20/Writing-Mathematic-Fomulars-in-Markdown/) pour le markdown et ce [site](https://en.wikibooks.org/wiki/LaTeX/Mathematics) plus généraliste. [site](https://openclassrooms.com/forum/sujet/formulaire-de-formules-latex-84687)

Expérience
----------

On va maintenant regarder l'exemple classique du "lancer de pièces" (flipping coins) a l'aide de R pour faire un peu plus de sens.

On présume que la pièce est bien équilibré et que la distribution des résultats est soumise à une distribution binomiale avec une chance d'obtenir le côté pile de 0.5 . La loi binomiale est la loi suivie par les résultats de tirages aléatoires lorsqu'il n'y a que deux possibilité mutiellement exclusives de résultat et que la probabilité d'obtenir chaque possibilité est constante au cours de l'expérience (*i.e.* population infinie ou tirage avec remise). La loi donne la probabilité d'obtenir *k* fois le résultat A quand *n* tirages sont réalisés. Ecriture : *B*(*n*, *p*) avec : - *n* le nombre de tirages - *p* probabilité assosciée au résultat A

Voir la fiche 20 de [l'aide mémoire](https://cran.r-project.org/doc/contrib/Herve-Aide-memoire-statistique.pdf)

``` r
# Génération de 2 ensembles pour 10 tirages, lancer la pièces 1 fois. l'événement E = 1 (pile) avec un probabilité de 0.5
Exp_A  <- rbinom(10, 1, .5)
Exp_B  <- rbinom(10, 1, .5)

print(paste0("Sample space of experiment A: ", list(Exp_A)))
```

    ## [1] "Sample space of experiment A: c(0, 1, 0, 1, 1, 0, 1, 1, 1, 0)"

``` r
print(paste0("Sample space of experiment B: ", list(Exp_B)))
```

    ## [1] "Sample space of experiment B: c(1, 0, 1, 1, 0, 1, 0, 0, 0, 1)"

``` r
cat("\n")
```

``` r
print(paste0("E compliment (tails flip) for sample A: ", list(!Exp_A)))
```

    ## [1] "E compliment (tails flip) for sample A: c(TRUE, FALSE, TRUE, FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, TRUE)"

``` r
print(paste0("E compliment (tails flip) for sample B: ", list(!Exp_B)))
```

    ## [1] "E compliment (tails flip) for sample B: c(FALSE, TRUE, FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, TRUE, FALSE)"

``` r
cat("\n")
```

``` r
print(paste0("Coin flip is 'heads' (1) for both sample A and B: ",
             list(Exp_A & Exp_B)))
```

    ## [1] "Coin flip is 'heads' (1) for both sample A and B: c(FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE)"

``` r
print(paste0("Coin flip is 'heads' (1) for either sample A or B: ",
             list(Exp_A | Exp_B)))
```

    ## [1] "Coin flip is 'heads' (1) for either sample A or B: c(TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE)"

``` r
cat("\n")
```

``` r
print(paste0("Coin flip is 'heads' (1) for sample A when B is 'tails': ",
             list(Exp_A & !Exp_B)))
```

    ## [1] "Coin flip is 'heads' (1) for sample A when B is 'tails': c(FALSE, TRUE, FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, TRUE, FALSE)"
