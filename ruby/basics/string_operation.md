
## opérations

### +/-/*

string + string == "string" + "string"

"string" * (interger)

string.even? == pair?

### ascii

letter.ord == asscii number dans un array

asscii_number.chr == asscii_number to string 

### compter le nbre de

string.count("aeiou") == compte nbre de voyelles 

### each_char

sorte de .map pour stringv == Each Character

string.each_char{|letter| letter....} == sort la nouvelle string

### égalité

string1 == string2

string1.eql?(string2) 

### index d'une lettre

string[index_number] == index d'une lettre

### inverser

string.reverse == string à l'envers

### longueur

string.length == nbre de lettre

### maj/min

string.upcase == tous en majuscule

string.downcase

string.capitalize == première lettre du mot en majuscule

string.swapcase == maj en min et min en maj

### match

string.match(pattern)
string.match(pattern,pos)

### mettre dans un array

string.split == string.split(' ') == met les lettres de la string dans un array 
string.split('') == string(//) == string.scan(/\w/) == mets chaque lettre de la phrase dans un array AVEC EXCEPTION CHARACTERES SPECIAUX

string.chars ~~ string.split('') == mets chaque element

### premier index/dernier

string[0]
string.start_with?(" ")

string[-1]
string.end_with?(" ")

### regex

string.match? la_regex 

### remplacement

==> gsub 

gsub(pattern, replacement) → new_str
gsub(pattern, hash) → new_str
gsub(pattern) {|match| block } → new_str
gsub(pattern) → enumerator 

==> tr 

string.tr(from_str, to_str) == remplace les from_str en to_str == sort une copie de string
string.tr!(from_str, to_str) == remplace la string de base
ex: ("hello").tr!('eo', 'an') == "halln" 

### string binary to integer

string.to_i(2) 

### supprimer

string.delete('x') == Returns a copy of str with all characters in the intersection of its arguments deleted.
==> string.delete("aeiou") == delete all the vowels of the string

string.delete_at(b.index(b.min)) == enlève le premier index de plus petite valeur

string.chop == supprime le dernier elt de la string

### to integer

Integer(string)
string.to_i 

### type element

string.class 

string.is_a?(String, Integer, Array...)

































