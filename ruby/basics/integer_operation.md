## opération

### +/-/*

integer + integer

integer - integer

integer * integer

integer / integer

integer.div(n) == integer/n

### ^

exclusif == sorte de !=

integer ^ integer

### arrondir

integer.round(n) == arrondir à n après la virgule

### ascii letters

integer.chr

### classer

integer.digits == mets chaque chiffre dans un array, dans l'ordre inverse

### conversion en binaire

	integer.to_s(2).to_i

OU 

	def binary(n)
		binary = Array.new 
		while n > 0
			binary << n % 2  # % == modulo
			n /= 2
		end 

		binary.reverse.join
	end


### comparaison

a <=> b == < || > || ==
ex: [0, 1, 2][a <=> b] == 0 si a < b, 1 si a > b, 2 si a == b

### float avec deux décimals

 '%.2f' % amount

### multiple

integer % num == 0 == integer multiple de num

### question

integer.even? == pair 
integer.odd? == impair
integer.between?(x, y) == compris entre
### racine carrée

Math.sqrt(interger)

### to string

String(integer)
integer.to_s
'#{integer}'

### valeur absolue

integer.abs

### Question

==> quel type d'element?

integer.is_a?(Integer)






