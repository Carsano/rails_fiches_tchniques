## Bases

Time.now == heure actuelle

## Opérations sur Time.now 

my_time = Time.now 

my_time.year == année de Time.now 

my_time.sec == ...


## +/-

Time compte toujorus en secondes

Time.now + 10 == + 10 SECONDES

Time.now + 3600 == + 1 heure du coup!!!

## parse

	require 'time' #retourne "true"

	Time.parse("2010-10-31 12:00") #=> 2010-10-31 12:00:00 +0100

## Affichage heur européenne

	my_time.strftime("%H:%M:%S %d/%m/%Y")