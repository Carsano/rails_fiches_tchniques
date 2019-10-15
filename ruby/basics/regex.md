
==> begin of regex

/

==> begin of string

\A

==> regex 

[a] == downcase

[z0-9] == number

[_] == contient underscore

{4,16} ==  between 4 and 16

==> end of string

\z

==> end of regex 

/

ex: if string contain low character, number, underscore, length between 4 and 16

/\A[a-z0-9_]{4,16}\z/