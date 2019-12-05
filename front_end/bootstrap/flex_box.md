https://mastery.games/p/flexbox-zombies

## Dans .css, cibler l'élément qui contenient les éléments

    element {
    display: flex;
    }

## checker si changement dans le front


## flex-direction

==> Définir si les éléments doivent être disposés en horizontal ou vertical

    - row: les éléments sont en lignes
    
    - row-reverse: les éléments sont en ligne, dans l'ordre inverse
    
    - column: les éléments sont mis en colonnes, de haut en bas
    
    - column-reverse: les éléments sont mis en place en colonnes, de bas en haut.


==> dans css

    element {
    display: flex;
    flex-direction: column;
    ...
    }


## justify-content

==> disposition des éléments horizontalement (flex-direction: row;) ou verticalement (flex-direction: column;)

    - start: éléments disposés au début

    - end: éléments disposés à la fin

    - center: éléments centrés
    
    - space-between: Items display with equal spacing between them.
    
    - space-around: Items display with equal spacing around them.

==> dans le css

    element {
        display: flex;
        justify-content: flex-start;
        ...
    }


## align-items

==> disposition des éléments sur l'axe vertical (row) et horizontal (column)

    - start: Items align to the top of the container.
    
    - end: Items align to the bottom of the container.
    
    - center: Items align at the vertical center of the container.

    - stretch: étire les éléments sur toute la hauteur, MIS PAR DEFAUT

    - between

    - around
    
    - baseline: Items display at the baseline of the container.
    
    - stretch: Items are stretched to fit the container.


==> dans le css

    element {
      display: flex;
      align-items: flex-start;
      ...  
    }

.target:nth-of-type(x){
}

.target.male{
  
}

# ATTENTION: quand flex-direction-column, align-items et justify content sont inversé!!!!

justify-content == aligns items vertically

align-items == aligne les items horizontalement


## align-self

==> sous-entend avoir cibler d'abord les éléments avec align-items

==> permet de cibler certains éléments dans les éléments et de les positionner autrement

==> même propriétés que align-items

ex: 

    element {
      display: flex;
      align-items: flex-start;
      ...  
    }

    element_cible {
      align-self: ...;
    }


## align-content

==> set how multiple lines are spaced apart from each other

    -start: Lines are packed at the top of the container.
    
    -end: Lines are packed at the bottom of the container.
    
    - center: Lines are packed at the vertical center of the container.

    - space-between: Lines display with equal spacing between them.

    - space-around: Lines display with equal spacing around them.
    
    - stretch: Lines are stretched to fit the container.


# ATTENTION: diiférence align-items et align-content

==> align-items == determines how the items as a whole are aligned within the container

==> align-content == determines the spacing between lines
When there is only one line, align-content has no effect.


## flex-direction

==> defines the direction items are placed in the container

    - row: Items are placed the same as the text direction.
    
    - row-reverse: Items are placed opposite to the text direction.
    
    - column: Items are placed top to bottom.
    
    - column-reverse: Items are placed bottom to top.


# ATTENTION: quand flex-direction-column, align-items et justify content sont inversé!!!!

justify-content == aligns items vertically

align-items == aligne les items horizontalement

## flex-wrap

==> to spread the items if too many

    - nowrap: Every item is fit to a single line.
    
    - wrap: Items wrap around to additional lines.
    
    - wrap-reverse: Items wrap around to additional lines in reverse.

## flex-flow

==> combine flex-direction et flex-wrap


## order (with item class)

==> to individual items

 By default, items have a value of 0, but we can use this property to also set it to a positive or negative integer value (-2, -1, 0, 1, 2).

==> align-self == same properties as align-items



