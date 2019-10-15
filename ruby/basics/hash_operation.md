## operation

### Create an hash

hash = Hash.new
hash = {}
hash = {key: 'value', 'key' => 'value'}

### fetch

==> fecth

hash.fetch('key') == give the value if exists

hash.fetch('key', "default") == set default if key doesn't exist

hash.fetch(key){|elt| key...elt}

==> fetch_values

hash.fetch_values(key) == give an array with the value of the key
