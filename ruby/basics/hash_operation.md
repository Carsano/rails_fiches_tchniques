## operation

### Create an hash

hash = Hash.new
hash = {}
hash = {key: 'value', 'key' => 'value'}
hash = {:key => value}

### trouver la key

hash.key(value)

###  value

==> trouver une ou plusieurs value
hash.value_at(key, ...)

==> y-a-t-il cette value?
hash.value?(value)

==> sortir les values dans un array
hash.values


### fetch

==> fecth

hash.fetch('key') == give the value if exists

hash.fetch('key', "default") == set default if key doesn't exist

hash.fetch(key){|elt| key...elt}

==> fetch_values

hash.fetch_values(key) == give an array with the value of the key

