

## justify-content

==> aligne les items horizontalement

    - start: Items align to the left side of the container.

    - end: Items align to the right side of the container.

    - center: Items align at the center of the container.
    
    - space-between: Items display with equal spacing between them.
    
    - space-around: Items display with equal spacing around them.


## align-items

==> aligns items vertically

    - start: Items align to the top of the container.
    
    - end: Items align to the bottom of the container.
    
    - center: Items align at the vertical center of the container.

    - between

    - around
    
    - baseline: Items display at the baseline of the container.
    
    - stretch: Items are stretched to fit the container.

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



