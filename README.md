# scrape-real-estate-property

Hello je suis heureux de te retrouver sur ce nouveaux tuto.
Aujourd'hui nous allons voir comment récuperer des données à partir d'un site internet.

Avant de chercher à scrapper un site web, il peut être intéressant de naviguer sur celui


Premier chose installer Jupyter Notebook : https://jupyter.org/install
<img width="1434" alt="Capture d’écran 2020-05-28 à 08 07 40" src="https://user-images.githubusercontent.com/16499678/83105171-74133b00-a0ba-11ea-8662-0753804230cb.png">

Puis :

- pip3 install requests

- pip3 install bs4 <---- = beautifulsoup4

### Puis on les importes dans Jupyter Notebook

<img width="873" alt="Capture d’écran 2020-05-28 à 08 12 54" src="https://user-images.githubusercontent.com/16499678/83105483-0d425180-a0bb-11ea-83ca-fe8a9973c072.png">

On retourne maintenant sur le site et la première question que vous devez vous poser c'est comment charger les données depuis site internet.

https://www.leboncoin.fr/recherche/?category=9&locations=Lieusaint_77127__48.63244_2.55128_3735&real_estate_type=1&rooms=4-max&price=125000-400000
ou
https://www.seloger.com/list.htm?projects=2,5&types=2&natures=1,2,4&places=[{ci:770251}]&price=230000/400000&bedrooms=4&enterprise=0&qsVersion=1.0
ou
http://www.pyclass.com/real-estate/rock-springs-wy/LCWYROCKSPRINGS/

J'ai fais un essaie sur leboncoin pour voir si ma connexion et ça à l'air de marché
<img width="872" alt="Capture d’écran 2020-05-28 à 09 03 27" src="https://user-images.githubusercontent.com/16499678/83109703-25699f00-a0c2-11ea-8a0f-b36bebc92b0d.png">

Je vais utiliser pour la demo, un site static qui ne bouge pas avec le temps, cela te permetrra de visionner les mêmes résultats que moi
<img width="864" alt="Capture d’écran 2020-05-28 à 09 32 01" src="https://user-images.githubusercontent.com/16499678/83112219-21d81700-a0c6-11ea-9008-3ab875fc9f1b.png">

On utilise beautiful soup pour rendre ces données plus lisible

`
r = requests.get("http://www.pyclass.com/real-estate/rock-springs-wy/LCWYROCKSPRINGS/", headers={'User-agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:61.0) Gecko/20100101 Firefox/61.0'})

c = r.content

soup=BeautifulSoup(c, "html.parser")

soup
`

On retroune maintenant sur le site et on va utiliser l'inspecteur d'élément pour checker comment récuperer nos biens (items)
<img width="1437" alt="Capture d’écran 2020-05-28 à 09 38 09" src="https://user-images.githubusercontent.com/16499678/83112876-1802e380-a0c7-11ea-9b10-5015213e483c.png">

Maintenant, on arrive bien à récuperer le prix mais le format ne nous convient pas. 
#1 Tu peux voir que ce qu'on récupère est une "string" cela veut dire qu'on peut appliqué des méthodes dessus

`type(all[0].find_all("h4", {"class":"propPrice"})[0].text)` retourn str, ce qui veut dire string

<img width="948" alt="Capture d’écran 2020-05-28 à 09 52 12" src="https://user-images.githubusercontent.com/16499678/83114179-eee35280-a0c8-11ea-818a-7ca17a1d4246.png">

On va utiliser la méthode .replace pour supprimer les éléments qui ne sont pas nécesssaire. Le premier paramètre est notre cible et le seconde, ce par quoi on veut le remplacer. Dans notre cas on va l'utiliser deux fois de suite

<img width="909" alt="Capture d’écran 2020-05-28 à 09 55 42" src="https://user-images.githubusercontent.com/16499678/83114577-6d3ff480-a0c9-11ea-9b42-c92ef6c811c8.png">

Pour que ça soit plus lisible j'ai fusionner les cellules
<img width="873" alt="Capture d’écran 2020-05-28 à 10 02 42" src="https://user-images.githubusercontent.com/16499678/83115306-77aebe00-a0ca-11ea-8da9-8dd5abd08e1f.png">

Bon maintenant qu'on a récuperer le première élément, il serait intéressant de récuperer tout les éléments de la liste. Pour cela nous avons besoins d'itérer 

<img width="906" alt="Capture d’écran 2020-05-28 à 10 07 44" src="https://user-images.githubusercontent.com/16499678/83115720-1804e280-a0cb-11ea-978a-5ddcf6debe5a.png">

Ok cool, maintenant récuperont l'adresse. Pour cela on peut encore une fois utiliser l'instpecteur d'élement pour checker ou elle se trouve. Je vois qu'elle est dans un span de class = propAddressCollapse. Sachant qu'il y a deux span, un qui contien le noms de la rue et l'autre qui contien la ville
<img width="1371" alt="Capture d’écran 2020-05-28 à 10 10 36" src="https://user-images.githubusercontent.com/16499678/83116065-919cd080-a0cb-11ea-90f0-ab4eb21522c3.png">

On va passer au nombre de bed, mais cette fois on va ce servire de try & except, qui vont nous servire à dire "si tu trouve ceci fait cela, sinon pas à l'étape suivante de notre loop. Voici ce que ça donne

<img width="995" alt="Capture d’écran 2020-05-28 à 10 31 36" src="https://user-images.githubusercontent.com/16499678/83118265-71224580-a0ce-11ea-99da-43d21a68d521.png">


Si l'on souhaite juste récuperer les nombres vous pouvez faire ceci

<img width="873" alt="Capture d’écran 2020-05-28 à 10 33 38" src="https://user-images.githubusercontent.com/16499678/83118520-c5c5c080-a0ce-11ea-89e9-7158f490828a.png">

On va récuprer d'autres détails et grace à la condition qu'on a decouvert avant, on va récuperer les données si il y en a pas tampis on passe , mais on ne se retrouve pas avaec une erreur
<img width="948" alt="Capture d’écran 2020-05-28 à 10 37 47" src="https://user-images.githubusercontent.com/16499678/83119007-716f1080-a0cf-11ea-972a-442a87274e0d.png">

Pour voir les éléments vide on peut demander à nous les afficher avec un print(None)
<img width="882" alt="Capture d’écran 2020-05-28 à 10 40 11" src="https://user-images.githubusercontent.com/16499678/83119187-af6c3480-a0cf-11ea-990b-8e0bf2ba3894.png">


## On part sur leboncoin
<img width="952" alt="Capture d’écran 2020-05-29 à 08 34 01" src="https://user-images.githubusercontent.com/16499678/83228799-2dd7dd80-a187-11ea-98c1-5ec8e7de5955.png">

Nous allons voir commment stocker le tout dans un fichier .CSV
On va commencer par stocker le tout dans un objet. On peut aussi réoganiser l'ordre, par exemple si on veut que le titre soit en premier. Ensuite on supprime le dernier print, ça donne ça.

<img width="972" alt="Capture d’écran 2020-05-29 à 08 44 44" src="https://user-images.githubusercontent.com/16499678/83229718-c458ce80-a188-11ea-9120-750340fadde4.png">

Maintenant on veut stocker nos annonces dans une liste. On créer un tableau ads=[] et à la fin de la boucle on stocke ad dans ads
<img width="970" alt="Capture d’écran 2020-05-29 à 08 48 22" src="https://user-images.githubusercontent.com/16499678/83229948-2f0a0a00-a189-11ea-9908-fba65dc936aa.png">

On peut regarder la longueur avec .len() et ensuite checker notre objet
<img width="989" alt="Capture d’écran 2020-05-29 à 08 51 30" src="https://user-images.githubusercontent.com/16499678/83230193-a475da80-a189-11ea-8af3-cf96d62bb1ba.png">
<img width="1256" alt="Capture d’écran 2020-05-29 à 08 51 42" src="https://user-images.githubusercontent.com/16499678/83230197-a63f9e00-a189-11ea-8005-36b3de7f4c54.png">

Maintenant pour l'importer dans un ficher .csv on a besoin de Panda.

pip3 install pandas

Puis 

import pandas
df = pandas.DataFrame(ads)
df

<img width="985" alt="Capture d’écran 2020-05-29 à 08 55 52" src="https://user-images.githubusercontent.com/16499678/83230568-3c73c400-a18a-11ea-9bbb-1c6bb7753567.png">


Ensuite pour télécharger le fichier sur notre machine. Il se retrouver dans le même dossier que notre projet
df.to_csv("annonces_immo.csv")

<img width="1185" alt="Capture d’écran 2020-05-29 à 08 58 50" src="https://user-images.githubusercontent.com/16499678/83230832-c2900a80-a18a-11ea-9a00-4c401315e8cf.png">

