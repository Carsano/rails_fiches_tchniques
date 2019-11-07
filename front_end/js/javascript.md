# ************* JAVASCRIPT ****************

==> toutes les opérations que l'on peut faire sur les objets js

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/

==> LA ressource où aller pour se mettre à jour sur JavaScript et être au fait des dernières tendances du langage

https://javascript.info/

==> track 

https://www.codecademy.com/catalog/subject/all 


## Règles de bases

### fichier de type script.js 

### script.js appelé dans le html par <sript src="script.js"></sript> juste avant le </html>

# lecture d'un fichier js DANS LE NAVIGATEUR WEB

### Ouvrir index.html dans le navigateur et faire Fn F12, puis onglet "console"

# console.log("ton_code") dans script.js == "puts" dans ruby

## Règles en plus


    - Chaque instruction en JS se termine par un point-virgule

    - JS n'est pas sensible aux espaces donc n'hésite pas à indenter ton code

    - Les commentaires s'écrivent avec // sur une ligne ou /* sur plusieurs lignes */ ainsi :

		    //commentaire sur une ligne
		    console.log("Ceci n'est pas un commentaire")
		    /* commentaire écrit sur 
		    plusieurs lignes*/

    - Les variables sont en général écrites en camelCase et en anglais : myVariable, numberOfPeople ou player1Score. La convention ici est moins forte qu'en Ruby : la seule règle stricte est qu'une variable ne doit pas commencer par un chiffre.

    - Tout comme en Ruby, les variables peuvent être de plusieurs types : string, number, boolean… Le type d'une variable myVariablepeut être identifié avec

# typeof myVariable;

    - Les opérateurs arithmétiques sont les mêmes qu'en Ruby : +, -, *, / et le modulo %.


## Variables

### Il existe trois types de déclaration de variable

==> let myVariable; == variable valable pour un bloc code

==> var myVariable; == variable valable pour le bloc code et le bloc principal

==> const myVariable; == variable dont on ne peut changer la valeur

### typeof myVariable == type de la variable == string, number, boolean…

### console.log(myVariable) == afficher la valeur de la variable dans la console du navigateur

### console.log(typeof myVariable) == afficher le type de ma variable

### Notion de bloc de code == {}

	let data1 = "variable en let définie dans le bloc principal";

	{ //début du sous-bloc

	  data1 = "variable en let modifiée dans le sous-bloc";
	  var data2 = "variable en var définie dans le sous-bloc";
	  let data3 = "variable en let définie dans le sous-bloc";

	} //fin du sous-bloc

	console.log(data1); // "variable let modifiée dans le sous-bloc"
	console.log(data2); // "variable var définie dans le sous-bloc"
	console.log(data3); // On aura une erreur : data3 n'est pas visible dans le bloc principal

# Préférer le let!!!! Evite tous conflits entre deux variables de même nom!

### Quelques manipulations de bases avec les variables:

==> Tout comme en Ruby, on peut incrémenter une variable avec ++ et +=.

	    let a = 1, b = 1;
	    a++
	    console.log(a);
	    console.log(b += 2);

==> Les opérateurs suivants marchent également: *=, /=, -= et %=

==> On peut concaténer deux strings avec +.

	    let a = "Bonjour", b = "Monde";
	    console.log(a + " " + b);

==> On peut convertir un string en number avec la fonction Number("5"). L'inverse est possible avec String(36). 

## Les arrays

Le fonctionnement des arrays en JS est très proche du Ruby.

==> Créer un array se fait de la façon suivante: 

let statusDeTHP = ["Moussaillon", "Corsaire", "Renégat"]; 

Bien évidemment, on peut mélanger différents types de variables dans un array : number, string, booléen, etc.

==> On accède à un élément de l'array comme en Ruby : la numérotation commence à zéro. Ainsi, statusDeTHP[0] retournera "Moussaillon".
    
==> La taille d'un array:

statusDeTHP.length;
    
==> Modifier la valeur d'une entrée: 

statusDeTHP[0] = "La moussaille"
    
==> On peut rajouter des entrées: 

statusDeTHP.push("Flibustier") == rajoute une entrée à la fin du array

statusDeTHP.unshift("Pirate") == rajoute une au début.
    
==> supprimer des élément: 

statusDeTHP.pop() == le dernier 

statusDeTHP.shift() == supprimera le premier 

statusDeTHP.splice(0,2) == enlèvera 2 éléments à partir de l'indice 0.
    
==> La fonction statusDeTHP.slice(2,4) te retourne un array issue du array initial (une sorte de sous-array). Ses valeurs seront celles comprises entre l'index 2 (inclus) et 4 (exclus).

## Les strings

En plus de la concaténation de base vue plus haut ("Bonjour" + " " + "Monde"), on peut faire les opérations suivantes sur un string :

==> On peut effectuer des conversions implicites, par exemple en faisant number + string on obtient un string. Exemple :

    let a = 3;
      console.log("Bonjour à tous les "+ a);
      console.log(a + "3");

# ==> Par contre JS n'accepte pas la syntaxe suivante let a = "coucou" * 2.

## .repeat

ex:

    let a = "coucou"

    a.repeat(2) = "coucou" * 2 !!!

==> On peut inclure la valeur d'une variable au sein d'un string en le déclarant ainsi: `ceci est la valeur de ma variable a : ${a}`. 

# Notez l'usage des ` ` pour identifier le string et de ${xx} pour identifier la variable à injecter dans le string. C'est l'équivalent du Ruby "coucou #{variable}".

Teste cela :

    let numStudent = 4;
    let statement = `Dans mon groupe on est ${numStudent} moussaillons`;
    console.log(statement);

==> Comme en Ruby, une chaîne de caractères peut être considérées comme un array contenant, dans chaque case, un caractère. Ainsi avec .length tu obtiens sa longueur, avec "The Hacking Project"[0] tu obtiens la lettre "T", etc.

==> Tu peux aller faire une recherche au sein du string avec .indexOf.

"The Hacking Project".indexOf("Hack") == 4 (le mot "Hack" commence à l'index 4 du string). 

Si tu obtiens -1 c'est que ça n'a pas été trouvé.

==> Les fonctions suivantes permettent de passer en minuscule / majuscule un string

toLowerCase() et toUpperCase()

==> split(",") == découpe un string en fonction d'un séparateur (la virgule ici) pour retourner un array de strings.

    const contentOfTHP = "Git-Ruby-Rails-HTML-CSS-JS";
    const lesson = contentOfTHP.split("-");
    console.log(lesson[0]); // "Git"
    console.log(lesson[5]); // "JS"

==> join(",") == l'exact inverse de split(","): elle produit un string en concaténant tous les éléments d'un tableau et en rajoutant, entre chaque élément, un séparateur (une virgule ici).


## Les objets littéraux

JavaScript est un langage orienté objet. Toutefois, ici, nous ne te présenterons pas le fonctionnement complet de la POO en JavaScript et allons nous contenter de parler des objets littéraux. Ils te donneront un micro aperçu de la POO en JS mais sans rentrer trop dans toute sa complexité.
Les objets littéraux, en JS, sont à mi-chemin entre un hash Ruby (on les définit sans créer de classe) et un objet Ruby (on peut leur définir des attributs et des fonctions).

==> On définit un objet JS de la façon suivante (exemple d'un objet comportant des infos sur la session 2 de THP) :

    let THPSessionNum2 = {
      numOfMouss: 80,
      # attribut: fonction
      cities: ["Paris", "Lyon", "Montpellier"],
      successRate: 0.80,
      sessionMoto: "keep pushing to the top"
    };

==> Une fois définis, on peut accéder aux attributs d'un objet en faisant myObject.attribut. Exemple (à partir de l'objet ci-dessus) :

    console.log(THPSessionNum2.numOfMouss);
    console.log(THPSessionNum2.sessionMoto);

==> On peut également modifier un attribut ou même en rajouter un avec myObject.attribut = xxxx. Exemple (à partir de l'objet ci-dessus):

    THPSessionNum2.numOfMouss = 79; 
    // je modifie un attribut existant

    console.log(THPSessionNum2.numOfMouss);

    THPSessionNum2.favoriteLesson = "Présentation de Sinatra" 
    //je rajoute un attribut

    console.log(THPSessionNum2); 
    //j'imprime tout l'objet pour voir

==> Tu peux également accéder aux attributs en utilisant la syntaxe myObject['attribut']. C'est très utile quand le nom de l'attribut est contenu dans une autre variable. Exemple à partir de l'objet ci-dessus :

    let attributName = "successRate";
    console.log(THPSessionNum2[attributName]);


## Les conditions

JavaScript gère les conditions à la sauce if, else et else if. Pour cela, il faut connaître les opérateurs de comparaison en JS : ils sont QUASIMENT identiques à ceux en Ruby:

    >= signifie "supérieur ou égal à". 

    > signifie strictement supérieur

    <= signifie "inférieur ou égal à". 

    < signifie strictement inférieur

    != signifie "contenu (= valeur) différent de"

    !== signifie "contenu ET type différent (string, integer, ...) de".

    == signifie "contenu différent de"

    === signifie "contenu ET type égal à".

ex:

	console.log("36" == 36); // va retourner TRUE car le contenu est identique

	console.log("36" === 36); // va retourner FALSE car d'un côté on a un string et de l'autre un number

Quand on débute, il est fortement conseillé de se contenter d'utiliser === sinon tu risques d'avoir des surprises.

De même, on a un opérateur != (compare le contenu) et un !== (compare le contenu et le type) : utilise !== au maximum quand tu commences en JS.


==> Un bloc if de base se construit ainsi :

    let number = 2; 
    if(number > 0) {
      console.log("number est positif");
    } 

==> On peut rajouter des résultats alternatifs avec else if et else. Exemple :

    let number = 0;
    if(number > 0) {
      console.log("number est positif");
    } else if(number === 0) {
      console.log("number est nul");
    } else {
      console.log("number est négatif");
    } 

==> Les opérateurs logiques en JS sont les mêmes qu'en Ruby : && signifie ET, || signifie OU et ! signifie NON. 

Exemples :

    if (true && true) {
      console.log("ce message s'affiche car avec un ET, si les deux conditions sont à TRUE, le résultat est TRUE");
    }
    if (true || false) {
      console.log("ce message s'affiche car avec un OU, si l'une des conditions est à TRUE, le résultat est TRUE");
    } 
    if (!false) {
      console.log("ce message s'affiche car un NON sur FALSE donne TRUE");
    }

==> Quand on a plusieurs scénarios possibles, on peut utiliser la condition switch … case. C'est l'équivalent JavaScript du case … when en Ruby. 
À noter que l'instruction break permet de sortir d'un case et que tous les cas non décrits dans les case finissent dans la partie default. Voici la syntaxe :

    weekNum = 1; //teste avec plusieurs valeurs
    switch (weekNum) {
      case 1:
        console.log("Semaine 1 - Introduction au Code");
        break;
      case 2:
        console.log("Semaine 2 - Découverte de Ruby");
        break;
      case 3:
        console.log("Semaine 3 - Programmation Orientée Objet");
        break;
      case 4:
        console.log("Semaine 4 - Initiation à Rails");
        break;
      case 5:
        console.log("Semaine 5 - Rails intermédiaire");
        break;
      case 6:
        console.log("Semaine 6 - Rails avancé");
        break;
      case 7:
        console.log("Semaine 7 - HTML / CSS et intro au JS");
        break;
      case 8:
        console.log("Semaine 8 - JavaScript de front");
        break;

      default:
        console.log("Cette entrée n'est pas reconnue");
        break;
    }

==> Dernier point important : comme en Ruby, certaines valeurs peuvent être évaluées à FALSE dans le code. Mais en Ruby, ça n'est valable que pour la valeur nil alors. 

# En JS, c'est le cas de la valeur undefined, du nombre 0 et du string vide "". 

- undifined variable == FALSE

- 0 == FALSE

- "" == FALSE

Illustration :

    let number = 0; //fais aussi le test avec un chiffre non nul
    if (number) {
      console.log("ce message ne s'affiche que si number est non nul ");
    }

    let string = ""; //fais aussi le test avec un mot non vide
    if (string) {
      console.log("ce message ne s'affiche que si string est non vide");
    }

    let myVariable // ici, myVariable sera undefined. Fais aussi le test en lui donnant une valeur: number, string, array ou autre.
    if (myVariable) {
      console.log("ce message ne s'affiche que si myVariable contient une valeur ");
    }

## Les boucles

Tout comme en Ruby, on peut faire des boucles en JS. Tu ne devrais pas être trop perdu : la logique est la même.

==> On peut faire des boucles for basées sur la valeur d'une variable (un compteur). La syntaxe est la suivante :

### for([initialisation de la variable]; [condition]; [incrémentation]) { };

    for(let count = 0; count <=5; count++){
      console.log(`on va compter jusqu'à 5 : ${count}`);
    }
    console.log(count); // Va créer une erreur car count est en let et donc son scope est limité au bloc for.
    //passe count en var pour tester ce que ça donne

### while([condition] { }; => tourne tant que la condition est TRUE. 

# prompt() == ouvre une fenêtre d'input dans ton navigateur (== gets.chomp en ruby)

    let answer = "";
    while(answer !== "oui") {
      console.log("alors ?")
      answer = prompt("Tu kiffes THP ?");
    } 
    console.log("haaa, tu nous fais plaisir !") // Qui se lassera le premier entre toi et ton ordi ? :D

==> Tu peux sortir d'une boucle while en utilisant un break. Exemple :

    let word = "";
    let letter;

    while (true) {
        letter = prompt('Tape UNE lettre stp :');

        if (letter) {
            word += letter; 
            //on rajoute la lettre saisie à la suite du mot

            console.log(word);
        } else { 
          // on rentre dans ce else si letter est vide (l'utilisateur ne saisit rien)

            break; 
            // On quitte la boucle
        }
    }

    console.log(`voilà ce que tu as tapé : ${word}`);


### On peut parcourir les array et les objets en JS grâce à l'instruction for in 

Pour un array, elle permet d'obtenir un compteur qui va prendre toutes les valeurs entre zéro et le dernier index.

Pour un objet, le compteur va prendre chaque valeur des attributs, une à une. 

Illustration :

    //On déclare d'abord un array
    let weeksOfTHPArray = ["Semaine 1 - Introduction au Code", "Semaine 2 - Découverte de Ruby", "Semaine 3 - Programmation Orientée Objet", "Semaine 4 - Initiation à Rails"];

    //On déclare ensuite un OBJET
    let weeksOfTHPObject = {Semaine5: "Rails intermédiaire", Semaine6: "Rails avancé", Semaine7: "HTML / CSS et intro au JS", Semaine8: "JavaScript de front"};

    console.log("**********Parcourons le array :")
    for(let index in weeksOfTHPArray) {
      console.log(index); // index va aller de 0 à 3
      console.log(weeksOfTHPArray[index]);
    }

    console.log("**********Parcourons l'objet :")
    for(let weekAttribut in weeksOfTHPObject) {
      console.log(weekAttribut); //weekAttribut va avoir les valeurs "Semaine5" à "Semaine8"
      console.log(weeksOfTHPObject[weekAttribut]);
    }

### .forEach() == ne fonctionne que sur les array!!! 

Il permet d'obtenir directement la valeur stockée à chaque index du array. 

Illustration en reprenant le array weeksOfTHPArray ci-dessus:
    
    //On déclare d'abord un array
    let weeksOfTHPArray = ["Semaine 1 - Introduction au Code", "Semaine 2 - Découverte de Ruby", "Semaine 3 - Programmation Orientée Objet", "Semaine 4 - Initiation à Rails"];

    console.log("**********Parcourons le array en forEach :")
    weeksOfTHPArray.forEach(weekContent => {
      console.log(weekContent)
    });


## Les fonctions

Évidemment, comme tout langage qui se respecte, on peut définir des fonctions en JS (l'équivalent de nos méthodes en Ruby). La syntaxe est relativement différente par contre :

### Une fonction se définit en faisant function nomDeMaFonction(){ //le code };. 

On l'appelle ensuite en faisant un nomDeMaFonction(). À noter qu'en JS, contrairement au Ruby, une fonction est toujours exécutée avec des parenthèses. 

Illustration d'une fonction de base:

    function sayHello() {
      console.log("Bonjour toi !");
    }

    sayHello();

### Comme en Ruby, on peut définir des données en entrée de la fonction et des données à retourner avec return. 

Exemple :

    function multiply(inputNumber1, inputNumber2) {
      let outputNumber = inputNumber1 * inputNumber2;
      return outputNumber;
    }

    console.log(multiply(2, 3));
    console.log(multiply(2, multiply(2,3)));
    console.log(outputNumber); // Va créer une erreur car la variable en let n'existe pas en dehors de la fonction

### JavaScript nous permet de déclarer d'autres façons nos fonctions. Par exemple par le biais de fonctions anonymes (sans nom) que l'on peut ensuite affecter à une variable en faisant myVariable = function(entrée1, ...){ }! C'est un mode de fonctionnement que tu n'as jamais vu en Ruby. 

Illustration :

    const multiply = function(inputNumber1, inputNumber2) {
      let outputNumber = inputNumber1 * inputNumber2;
      return outputNumber;
    }

    console.log(multiply(2, 3));

    const otherMultiply = multiply; //On peut ainsi affecter la fonction à une autre variable
    console.log(otherMultiply(2, 3));

==> Enfin, sachez que depuis les dernières mises à jour ES, on peut également déclarer les fonctions anonymes de façon plus épurée avec l'opérateur =>. Pour reprendre l'exemple ci-dessus, cela donnerait :

    const multiply = (inputNumber1, inputNumber2) => {
      let outputNumber = inputNumber1 * inputNumber2;
      return outputNumber;
    }









