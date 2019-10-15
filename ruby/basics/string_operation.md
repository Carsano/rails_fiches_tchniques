
## opérations

### kind of element

string.class 

string.is_a?(String, Integer, Array...)

### longueur
string.length == nbre de lettre

### premier index/dernier

string[0]
string.start_with?(" ")

string[-1]
string.end_with?(" ")

### each_char

sorte de .map pour stringv == Each Character

string.each_char{|letter| letter....} == sort la nouvelle string

### compter le nbre de

string.count("aeiou") == compte nbre de voyelles

### index d'une lettre

string[index_number] == index d'une lettre

### +/-/*

string + string == "string" + "string"

"string" * (interger)

string.even? == pair?

### mettre dans un array

string.split == string.split(' ') == met les lettres de la string dans un array 
string.split('') == string(//) == string.scan(/\w/) == mets chaque lettre de la phrase dans un array AVEC EXCEPTION CHARACTERES SPECIAUX

string.chars ~~ string.split('') == mets chaque element

### maj/min

string.upcase == tous en majuscule
string.downcase
string.capitalize == première lettre du mot en majuscule

### supprimer

string.delete('x') == Returns a copy of str with all characters in the intersection of its arguments deleted.
==> string.delete("aeiou") == delete all the vowels of the string

string.delete_at(b.index(b.min)) == enlève le premier index de plus petite valeur

string.chop == supprime le dernier elt de la string

### inverser

string.reverse == string à l'envers

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

### to integer

Integer(string)
string.to_i

### regex

string.match? la_regex

### ascii

letter.ord == asscii number dans un array

asscii_number.chr == asscii_number to string


### nouvelle string

string.gsub(cible, change) == create a substring who can be modified

	gsub(pattern, replacement) → new_str click to toggle source
	gsub(pattern, hash) → new_str
	gsub(pattern) {|match| block } → new_str
	gsub(pattern) → enumerator 

string.match(pattern)
string.match(pattern,pos)

### égalité

string1 == string2

string1.eql?(string2)

### string binary to integer

string.to_i(2)