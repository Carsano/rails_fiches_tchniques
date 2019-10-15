## BASES

### création d'array

==> Array.new

==> a = []

==> [1, "items"]

==> a = [*1..5] == [1, 2, 3, 4, 5]

==> Array(1..n) == [1, 2, 3, 4, n]

### insertion array

a = []

a << 1

a.push(element)

### position array index

array = [0, 1, 2, 3, 4] OU [0, 1, 2, -2, -1]
# D'où array.first == array[0] && array.last == array[-1]



## opérations

# différence entre ! et pas ==> ! ne sort pas un autre array

### 2 arrays in one hash

Hash.[array_1.zip(array_2)] == 2 arrays in one hash

### additionner/soustraire/multiplier

array.sum == sum of values of array
array.inject(&:+)
array.reduce(:+)

array.inject(&:-)
array.reduce(:-)

array.inject(&:*) == array.inject(&:*)
array.reduce(:*)

### appliquer à chaque elts

array.map == appliquer une fonction à chaque élément de l'array

array.map{|x| x...} == kind of each do, creating a new array
# ou array.map(&:...)

array.map!{|x| x...} == replacing the array

array.collect{|x| x...} == create a new arrays

### boucle each

array.each do |x| x end == array.each {|x| x....}
array.each_with_index do |x, i| x, i end

### classer

array.sort == alphabetical order
array.sort_by {|x| x..} == trier par ... dans l'ordre

array.first == first elt of the array
array.last

array.min == sort l'element premier en fonction de l'alphabet
array.max ==                dernier

### compter

array.count
array.length
array.size

array.count('what_we_look_for')

### concertir en string

array.join(' ') == array to string with space
array.join('***')== array to string with *** between elements

### division

if x % nbre = 0 ==> x multiple de nbre

### inverser

array.reverse

### match

.match == regex

### max/min si chiffre

array.max
array.min

### opérations entre arrays

#### +/-

ex arr1 = [1, 2, 3, 4, 5] et arr2 = [2, 3]

arr1 + arr2 = [1, 2, 3, 4, 5, 2, 3] == arr1.concat(arr2)

arr1 - arr2 = [1, 4, 5]

arr2 - arr1 = []

### partitionner == partition

animals = ["cat", "dog", "duck", "cow", "donkey"]
partition(animals){|animal| animal.size == 3}
    #=> [["cat", "dog", "cow"], ["duck", "donkey"]]

### question == boolean(true par défaut)

==> array vide???

array.empty? (true par défaut)

==> tous les elments?

array.all?

==> tous les elments négatifs?

array.all?(&:negative?)

==> tous les elments positifs?

array.all?(&:positive)

==> est-ce-que l'array contient?

array.include?(element)

### remplacer

array.replace([....]) == replace content of an array with a new one

### selectionner

array.select {|item| block } == get the item selected
array.select

### si array dans array en un seul

array.flatten

array.flatten(index_de l'array qu'on veut flatten) 

### sortir un index

array.rindex()

array.at(index) == ressort l'élément de l'index

### supprimer

array.clear == supprime l'array

array.delete_if{|x| x...} == delete elts in array if x ...
array.delete_at(index) == delete the elt index

# attention: extrait dans un array l'index supprimé!!!
# remettre array après pour avoir l'array sans l'element supprimé!!!

array.delete(object)

array.uniq == supprrime les elts en double

array.reject{|x| x...}

### with_index

permet d'inclure l'index de l'elt

ex: array.with_index {|x, i| x.. i ..}



