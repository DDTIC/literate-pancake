
# Les tests en python



## Table des matières

* [Introduction](#intro)
* [Tester votre code](#test_code)
    * [Tests automatisés ou manuels](#auto_manuel)
    * [Tests unitaires vs tests d'intégration](#unit_inte)
    * [Choisir un testeur](#choix_testeur)
* [Écrire votre premier test](#premier_test)
    * [Où écrire le test](#ouecrire)
    * [Comment structurer un test simple](#structure_test)
    * [Comment écrire des assertions](#assertion)
    * [Effets secondaires](#effet_secondaire)
* [Exécution de votre premier test](#execution)
    * [Exécution des testeurs](#execution_testeur)
    * [Comprendre la sortie de test](#sortie_test)
    * [Exécuter vos tests depuis PyCharm](#test_pycharme)
    * [Exécution de vos tests à partir de Visual Studio Code](#test_vscode)
* [Ecrire des tests avec des frameworks Web comme Django et Flask](#frameworks)
    * [Pourquoi elles sont différentes des autres applications](#difference)
    * [Comment utiliser l'exécuteur de test Django](#executeur_django)
    * [Comment utiliser unittest et Flask](#unitest_flask)
* [Scénarios de test plus avancés](#scenario)
    * [Gestion des échecs attendus](#echecs)
    * [Isoler les comportements dans votre application](#isoler)
    * [Rédaction de tests d'intégration](#test_integration)
    * [Tester des applications basées sur les données](#test_donnees)
* [Tests dans plusieurs environnements](#environnement)
    * [Installation de Tox](#tox)
    * [Configuration de Tox pour vos dépendances](#config_tox)
    * [Exécuter Tox](#executer_tox)
* [Automatiser l'exécution de vos tests](#test_auto)
* [Et après](#apres)
    * [Introduire des linters dans votre application](#linter)
    * [Garder votre code de test propre](#test_propre)
    * [Test de la dégradation des performances entre les modifications](#degradation)
    * [Test des failles de sécurité dans votre application](#securite)
* [Conclusion](#conclusion)
* [Bibliographie](#biblio)


## Introduction  <a class="encre" id="intro"></a>
Les tests en Python sont un sujet énorme et peuvent avoir l'ère d' être très complexes à mettre en oeuvre, mais cela n'est pas le cas. Le programmeur python peut commencer à créer des tests simples pour son application en quelques étapes simples, puis les développer à partir de là.

Dans ce article, nous apprendrons à créer un test de base, à l'exécuter et à trouver les bogues avant nos utilisateurs ! nous découvriront les outils disponibles pour écrire et exécuter des tests, vérifier les performances de notre application et même rechercher des problèmes de sécurité.

## Tester votre code  <a class="encre" id="test_code"></a>
    
   Il existe de nombreuses façons de tester un code en python.  Nous apprendrons les  
   techniques des étapes les plus élémentaires et travaillerons vers des méthodes avancées.
    
### Tests automatisés ou manuels <a class="encre" id="auto_manuel"></a>
    
   La bonne nouvelle est que vous avez probablement déjà créé un test sans vous en rendre   
   compte. Vous souvenez-vous quand vous avez exécuté votre application et que vous l'avez    
   utilisée pour la première fois ? Avez-vous vérifié les fonctionnalités et expérimenté leur  
   utilisation ? C'est ce qu'on appelle les *tests exploratoires* et c'est une forme de test manuel.

   Les tests exploratoires sont une forme de test qui se fait sans plan. Dans un test exploratoire,  
   vous explorez simplement l'application.

   Pour disposer d'un ensemble complet de tests manuels, il vous suffit de dresser une liste de 
   toutes les fonctionnalités de votre application, des différents types d'entrées qu'elle peut 
   accepter et des résultats attendus. Désormais, chaque fois que vous modifiez votre code, 
   vous devez parcourir chaque élément de cette liste et le vérifier.

   Cela ne semble pas très amusant, n'est-ce pas?

   C'est là qu'interviennent les tests automatisés. Les tests automatisés sont l'exécution de 
   votre plan de test (les parties de votre application que vous souhaitez tester, l'ordre dans l
   equel vous souhaitez les tester et les réponses attendues) par un script au lieu d'un humain . 
   Python est déjà livré avec un ensemble d'outils et de bibliothèques pour vous aider à créer 
   des tests automatisés pour votre application. Nous allons explorer ces outils et bibliothèques 
   dans ce article.
    
### Tests unitaires vs tests d'intégration <a class="encre" id="unit_inte"></a>
    
   Le monde des tests ne manque pas de terminologie, et maintenant que vous connaissez la  
   différence entre les tests automatisés et manuels, il est temps d'aller plus loin.
    
   Pensez à la façon dont vous pourriez tester les phares d'une voiture. Vous allumez les 
   lumières (connu sous le nom d'étape de test ) et sortez de la voiture ou demandez à un ami 
   de vérifier que les lumières sont allumées (connu sous le nom d'assertion de test ). Le test de 
   plusieurs composants est appelé test d'intégration .
    
   Pensez à toutes les choses qui doivent fonctionner correctement pour qu'une tâche simple 
   donne le bon résultat. Ces composants sont comme les parties de votre application, toutes 
   ces classes, fonctions et modules que vous avez écrits.
    
   Un défi majeur avec les tests d'intégration est lorsqu'un test d'intégration ne donne pas le 
   bon résultat. Il est très difficile de diagnostiquer le problème sans pouvoir isoler quelle partie 
   du système est défaillante. Si les lumières ne s'allument pas, les ampoules sont peut-être 
   cassées. La batterie est-elle morte ? Et l'alternateur ? L'ordinateur de la voiture est-il en 
   panne ?
   
   Si vous avez une voiture moderne et élégante, elle vous dira quand vos ampoules ont disparu. 
   Il le fait en utilisant une forme de test unitaire .
   
   Un test unitaire est un test plus petit, qui vérifie qu'un seul composant fonctionne 
   correctement. Un test unitaire vous aide à isoler ce qui est cassé dans votre application et à le 
   réparer plus rapidement.
   
   Vous venez de voir deux types de tests :
   
 - Un test d'intégration vérifie que les composants de votre application fonctionnent les uns avec les autres.
     
 - Un test unitaire vérifie un petit composant de votre application.
   
   Vous pouvez écrire à la fois des tests d'intégration et des tests unitaires en Python. Pour 
   écrire un test unitaire pour la fonction intégrée sum(), vous devez vérifier la sortie de sum() 
   par rapport à une sortie connue.
  
   Par exemple, voici comment vous vérifiez que la somme des nombres (2, 2, 3) est égal à 7 :
   
   ```python
   assert sum([2,2,3]) == 7, "doit être 7"
   ```
   
   cela n'affichera rien car les valeurs sont correctes. 
   Si le résultat de sum() est incorrect, cela echouera avec un *AssertionError* et le message 
   "doit être 7"
   
    >>> assert sum([2, 4, 5]) == 7, "doit être 7"
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    AssertionError: doit être 7
  
  Voyons comment ecrire une fonction pour faire notre test :
  ```python
  def test_sum():
    assert sum([2,2,3]) == 7, "doit être 7"
   
   if __name__ == "main":
     test_sum()
     print("Tout est passé !")
  ```
  
  Nous avons maintenant écrit un cas de test , une assertion et un point d'entrée. Nous pouvons maintenant exécuter ceci et on verra le message "Tout est passé !"  car le résultat est exacte.
  
  En Python, sum() accepte tout itérable comme premier argument. Nous avons testé avec une liste. Maintenant, testons également avec un tuple. Créez un nouveau fichier appelé test_sum_2.py avec le code suivant :
  
  ```python
  def test_sum():
    assert sum([2, 2, 3]) == 7, "doit être 7"

def test_sum_tuple():
    assert sum((4, 2, 2)) == 7, "doit être 7"

if __name__ == "__main__":
    test_sum()
    test_sum_tuple()
    print("Tout est passé !")
    
  ```
  
  Lorsque vous exécutez test_sum_2.py, le script génère une erreur car la somme de (4, 2, 2) est 8, et non 7. Le résultat du script vous donne le message d'erreur :
  
    Traceback (most recent call last):
    File "/Users/gredey/Documents/test_sum2.py", line 9, in <module>
    test_sum_tuple()
    File "/Users/gredey/Documents/test_sum2.py", line 5, in test_sum_tuple
    assert sum((4, 2, 2)) == 7, "doit être 7"
    AssertionError: doit être 7

   
Ici, vous pouvez voir comment une erreur dans votre code génère une erreur sur la console avec des informations sur l'emplacement de l'erreur et le résultat attendu.

Écrire des tests de cette manière est acceptable pour une simple vérification, mais que se passe-t-il si plusieurs échouent ? C'est là qu'interviennent les *test runners* ou lanceurs de test. Le test runner est une application spéciale conçue pour exécuter des tests, vérifier la sortie et vous donner des outils pour déboguer et diagnostiquer les tests et les applications.

### Choisir un testeur <a class="encre" id="choix_testeur"></a>

Il existe de nombreux testeurs disponibles pour Python. Celui intégré à la bibliothèque standard Python s'appelle unittest. Dans ce article, nous utiliserons des scénarios de test *unittest* et *unittest test runner*. Les principes de unittest sont facilement transférables à d'autres frameworks. Les trois test runner les plus populaires sont :

  * unittest
  * nose ou nose2
  * pytest
  
Il est important de choisir le meilleur testeur pour vos besoins et votre niveau d'expérience.

#### unittest

unittest est intégré à la bibliothèque standard Python depuis la version 2.1. Vous le verrez probablement dans les applications Python commerciales et les projets open source.

unittest contient à la fois un framework de test et un test runner. unittest a des exigences importantes pour l'écriture et l'exécution des tests.

unittest nécessite que :
   * Vous mettez vos tests dans des classes en tant que méthodes
   * Vous utilisez une série de méthodes d'assertion spéciales dans la classe unittest.TestCase au lieu des assertions
 
 Pour convertir l'exemple précédent en cas de test unittest, vous devez :
   * Importer unittest depuis la bibliothèque standard
   * Créer une classe appelée TestSum qui hérite de la  classe TestCase
   * Convertissez les fonctions de test en méthodes en ajoutant self comme premier argument
   * Modifier les assertions pour utiliser la méthode self.assertEqual() de la classe TestCase
   * Appeler unittest.main() dans le main
   
   Si vous exécutez ceci sur la ligne de commande, vous verrez un succès (indiqué par .) et un 
   échec (indiqué par F):
   
    .F
        ======================================================================
              FAIL: test_sum_tuple (__main__.TestSum)
    ----------------------------------------------------------------------
     Traceback (most recent call last):
    File "/Users/gredey/Documents/test_unitest0.py", line 10, in test_sum_tuple
    self.assertEqual(sum((1, 2, 2)), 7, "doit être 7")
    AssertionError: 5 != 7 : doit être 7

    ----------------------------------------------------------------------
    Ran 2 tests in 0.000s

    FAILED (failures=1)
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % 

Nous venons d'exécuter deux tests à l'aide du test runner de unittest.

    Remarque : Soyez prudent si vous écrivez des cas de test qui doivent être exécutés à 
    la fois en Python 2 et 3. 
    En Python 2.7 et versions antérieures, unittest s'appelle unittest2. 
    Si vous importez simplement depuis unittest,vous obtiendrez différentes versions 
    avec différentes fonctionnalités entre Python 2 et 3.
     
 
 Pour plus d'informations sur unittest, vous pouvez explorer la [documentation de unittest](https://docs.python.org/3/library/unittest.html)
 
#### nose

Vous constaterez peut-être qu'au fil du temps, alors que vous écrivez des centaines, voire des milliers de tests pour votre application, il devient de plus en plus difficile de comprendre et d'utiliser la sortie de unittest.

nose est compatible avec tous les tests écrits à l'aide du framework  unittest et peut être utilisé en remplacement de l' unittest test runner. Le développement de nose en tant qu'application open source a pris du retard et un fork appelé nose2 a été créé. Si vous partez de zéro, il est recommandé d'utiliser à la place de nose, nose2.

Pour commencer avec nose2, installez nose2 à partir de PyPI et exécutez-le sur la ligne de commande. nose2 essaiera de découvrir tous les scripts de test nommés test*.py et les cas de test dont ils héritent de unittest.TestCase  dans votre répertoire actuel :

    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % pip3 install nose2
    Collecting nose2
    Downloading nose2-0.11.0-py2.py3-none-any.whl (144 kB)
     |████████████████████████████████| 144 kB 113 kB/s 
    Collecting coverage>=4.4.1
    Downloading coverage-6.4.1-cp39-cp39-macosx_10_9_x86_64.whl (184 kB)
     |████████████████████████████████| 184 kB 56 kB/s 
    Requirement already satisfied: six>=1.7 in /Users/gredey/opt/anaconda3/lib/python3.9/site-packages (from nose2) (1.16.0)
    Installing collected packages: coverage, nose2
    Successfully installed coverage-6.4.1 nose2-0.11.0
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % python3 -m nose
      .F
    ======================================================================
            FAIL: test_sum_tuple (test_unitest0.TestSum)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
    File "/Users/gredey/Documents/test_unitest0.py", line 10, in test_sum_tuple
    self.assertEqual(sum((1, 2, 2)), 7, "doit être 7")
    AssertionError: 5 != 7 : doit être 7

    ----------------------------------------------------------------------
    Ran 2 tests in 0.065s

    FAILED (failures=1)
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % 

 Nous venons d'exécuter le test que nous avons créé  test_sum_unittest.py à partir du testeur nose2. nose2 propose de nombreux indicateurs de ligne de commande pour filtrer les tests que vous exécutez. Pour plus d'informations, vous pouvez explorer la [documentation de Nose 2](https://docs.nose2.io/en/latest/) .
 
#### pytest
 
 pytest prend en charge l'exécution des cas de test unittest. Le véritable avantage de pytest vient en écrivant des cas de test pytest.  Les cas de test pytest sont une série de fonctions dans un fichier Python commençant par le nom *test_.*

pytest possède d'autres fonctionnalités intéressantes :

  * Prise en charge de l'instruction intégrée  assert au lieu d'utiliser des méthodes spéciales  self.assert*()
  * Prise en charge du filtrage des cas de test
  * Possibilité de relancer à partir du dernier test ayant échoué
  * Un écosystème de centaines de plugins pour étendre les fonctionnalités

L' exemple de cas de test de TestSum avec pytest ressemblerait à ceci :

```python
def test_sum():
    assert sum([2, 2, 3]) == 7, "doit être 7"

def test_sum_tuple():
    assert sum((4, 2, 2)) == 7, "doit être 7"
```

Nous avons supprimé le TestCase, toute utilisation de classes et le point d'entrée de la ligne de commande.

Plus d'informations peuvent être trouvées sur le site Web de [documentation de Pytest](https://docs.pytest.org/en/latest/) .


## Écrire votre premier test <a class="encre" id="premier_test"></a>

Rassemblons ce que nous avons appris jusqu'à présent et, au lieu de tester la fonction sum() intégrée, testons une implémentation simple de la même exigence.

Créez un nouveau dossier de projet et, à l'intérieur de celui-ci, créez un nouveau dossier appelé ma_somme. A l' intérieur de ma_somme, créez un fichier vide appelé  __init__.py. La création du fichier __init__.py  signifie que le dossier ma_somme  peut être importé en tant que module à partir du répertoire parent.

Votre dossier de projet devrait ressembler à ceci :

    project/
    │
    └── ma_somme/
       └── __init__.py
       
 Ouvrez ma_somme/__init__.py et créez une nouvelle fonction appelée somme(), qui prend un 
 itérable (une liste, un tuple ou un ensemble) et additionne les valeurs :
 
 ```python
     def somme(arg):
     total = 0
     for val in arg:
        total += val
        return total
 ```
 
 Cet exemple de code crée une variable appelée total, parcourt toutes les valeurs de arg et les ajoute à total. Il renvoie ensuite le résultat une fois l'itérable épuisé.
 
###  Où ecrire le test <a class="encre" id="ouecrire"></a> 
   
   Pour commencer à écrire des tests, vous pouvez simplement créer un fichier appelé test.py,  
   qui contiendra votre premier cas de test. Étant donné que le fichier devra pouvoir importer 
   votre application pour pouvoir la tester, vous souhaitez placer test.py au-dessus du dossier 
   du package, de sorte que votre arborescence de répertoires ressemblera à ceci :
   
     project/
     │
     ├── ma_somme/
     │   └── __init__.py
     |
     └── test.py
     
   Vous constaterez que, au fur et à mesure que vous ajoutez de plus en plus de tests, votre fichier unique deviendra encombrant et difficile à maintenir, vous pouvez donc créer un dossier appelé tests et diviser les tests en plusieurs fichiers. Il est de convention de s'assurer que chaque fichier commence par test_ afin que tous les testeurs supposent que le fichier Python contient des tests à exécuter. Certains très grands projets divisent les tests en plusieurs sous-
 répertoires en fonction de leur objectif ou de leur utilisation.
 
     Remarque : Que se passe-t-il si votre application est un    
     script unique ?

    Vous pouvez importer tous les attributs du script, tels que 
    les classes, les fonctions et les variables à l'aide de la  
    fonction __import__() intégrée. Au lieu de from ma_somme 
    import sum, vous pouvez écrire ce qui suit :
    
```python
          target = __import__("ma_somme.py")
          somme = target.somme
```
    L'avantage de l'utilisation __import__()est que vous n'avez 
    pas à transformer votre dossier de projet en package et vous 
    pouvez spécifier le nom du fichier. Ceci est également utile 
    si votre nom de fichier entre en collision avec des 
    packages de bibliothèque standard. Par exemple, math.py 
    entrerait en collision avec le module math .
    
###  Comment structurer un test simple <a class="encre" id="structure"></a>  

Avant de vous plonger dans l'écriture de tests, vous voudrez d'abord prendre quelques décisions :

  1. Que veux-tu tester ?
  2. Êtes-vous en train d'écrire un test unitaire ou un test d'intégration ?

Ensuite, la structure d'un test devrait suivre vaguement ce flux de travail :

  1. Créez vos entrées
  2. Exécuter le code testé, en capturant la sortie
  3. Comparer la sortie avec un résultat attendu
   
   Pour cette application, vous testez sum(). Vous pouvez vérifier de nombreux comportements de sum(), tels que :

  * Peut-il additionner une liste de nombres entiers (entiers) ?
  * Peut-il résumer un tuple ou un ensemble ?
  * Peut-il résumer une liste de flotteurs ?
  * Que se passe-t-il lorsque vous lui fournissez une mauvaise valeur, comme un seul entier ou une chaîne ?
  * Que se passe-t-il lorsque l'une des valeurs est négative ?
  
Le test le plus simple serait une liste d'entiers. Créez un fichier, test.py avec le code Python suivant :

```python
import unittest

from ma_somme import somme


class TestSum(unittest.TestCase):
    def test_list_int(self):
        """
        tester qu'il peut additionner une liste d'entiers
        """
        donnee = [3, 2, 2]
        res = somme(donnee)
        self.assertEqual(res, 7)

if __name__ == '__main__':
    unittest.main()
```
   

Cet exemple de code :

1. Importations de somme() depuis le package ma_somme  que vous avez créé

2. Définit une nouvelle classe de cas de test appelée TestSum, qui hérite de unittest.TestCase

3. Définit une méthode de test, .test_list_int(), pour tester une liste d'entiers. La méthode .test_list_int() va :

   * Déclarer une variable data avec une liste de nombres(3, 2, 2)
   * Affecter le résultat de ma_somme.somme(donnee) à une variable res 
   * Affirmer que la valeur de res est egal à 7 en utilisant la méthode .assertEqual() sur la classe unittest.TestCase 
4. Définit un point d'entrée de ligne de commande, qui exécute le test-runner unittest .main()

### Comment écrire des assertions <a class="encre" id="assertion"></a>

   La dernière étape de l'écriture d'un test consiste à valider la sortie par rapport à une réponse connue. C'est ce qu'on appelle une affirmation . Il existe quelques bonnes pratiques générales concernant la rédaction des assertions :
   
   * Assurez-vous que les tests sont reproductibles et exécutez votre test plusieurs fois pour vous assurer qu'il donne le même résultat à chaque fois
   * Essayez d'affirmer les résultats qui se rapportent à vos données d'entrée, par exemple en vérifiant que le résultat est la somme réelle des valeurs dans l'exemple somme() 
   
unittest est livré avec de nombreuses méthodes pour affirmer les valeurs, les types et l'existence des variables. Voici quelques-unes des méthodes les plus couramment utilisées :

| Method                              | Equivalent to         |
|-----------------------   |------------------
|   .assertEqual(a, b)           | a == b                    |
|   .assertTrue(x)                  | bool(x) is true      | 
|   .assertFalse(x)                 | bool(x) is false    | 
|   .assertIs(a, b)                   | a is b                      |
|   .assertIsNone(x)              | x is none              |
|   .assertIn(a, b)                   | a in b                      |
|   .assertIsInstance(a, b)    | isinstance(a, b)   |

.assertIs(), .assertIsNone(), .assertIn(), et .assertIsInstance() ont tous des méthodes opposées, nommées .assertIsNot(), et ainsi de suite.

### Effets secondaires <a class="encre" id="effet_secondaire"></a>

   Lorsque vous écrivez des tests, ce n'est souvent pas aussi simple que de regarder la valeur 
   de retour d'une fonction. Souvent, l'exécution d'un morceau de code modifie d'autres 
   éléments de l'environnement, tels que l'attribut d'une classe, un fichier sur le système de 
   fichiers ou une valeur dans une base de données. Ceux-ci sont connus sous le nom d' effets 
   secondaires et constituent une partie importante des tests. Décidez si l'effet secondaire est 
   testé avant de l'inclure dans votre liste d'affirmations.

  
  Si vous constatez que l'unité de code que vous souhaitez tester a de nombreux effets   
  secondaires, vous enfreignez peut-être le principe de responsabilité unique . Briser le principe 
  de responsabilité unique signifie que le morceau de code fait trop de choses et qu'il vaudrait 
  mieux qu'il soit refactorisé. Suivre le principe de responsabilité unique est un excellent moyen 
  de concevoir du code qui facilite l'écriture de tests unitaires simples et répétables pour des 
  applications fiables.
  
  
##  Exécution de votre premier test <a class = "encre" id="execution"></a>
 
  Maintenant que vous avez créé le premier test, vous souhaitez l'exécuter. Bien sûr, vous savez que cela va réussir, mais avant de créer des tests plus complexes, vous devez vérifier que vous pouvez exécuter les tests avec succès.
  
### Exécution des testeurs <a class="encre" id="execution_testeur"></a>
   
   L'application Python qui exécute votre code de test, vérifie les assertions et vous donne les résultats des tests dans votre console,  est appelée test runner .

Au bas de test.py, vous avez ajouté ce petit extrait de code :

```python
if __name__ == '__main__':
    unittest.main()
```

Il s'agit d'un point d'entrée de la ligne de commande. Cela signifie que si vous exécutez le script  python seul test.py sur la ligne de commande, il appellera unittest.main(). Cela exécutera le lanceur de test en découvrant toutes les classes de ce fichier qui héritent de unittest.TestCase.

C'est l'une des nombreuses façons d'exécuter le testeur unittest . Lorsque vous avez un seul fichier de test nommé test.py, l'appel python test.py est un excellent moyen de commencer.

Une autre méthode consiste à utiliser la ligne de commande unittest . Essaye ça:

    python -m unittest test
   
 Cela exécutera le même module de test (appelé test) via la ligne de commande.

Vous pouvez fournir des options supplémentaires pour modifier la sortie. L'un d'eux est -v pour verbeux. Essayez cela ensuite :

    python -m unittest -v test
 
 Cela va exécuter le seul test à l'intérieur de test.py et afficher les résultats sur la console. Le mode verbeux répertorie les noms des tests exécutés en premier, ainsi que le résultat de chaque test.

Au lieu de fournir le nom d'un module contenant des tests, vous pouvez demander une découverte automatique en utilisant ce qui suit :

    python -m unittest discover
    
 Cela recherchera dans le répertoire courant tous les fichiers nommés test*.py et tentera de les  tester.

Une fois que vous avez plusieurs fichiers de test, tant que vous suivez le modèle de nommage de test*.py , vous pouvez fournir le nom du répertoire à la place en utilisant le paramètre -s et le nom du répertoire :

    python -m unittest discover -s tests
    
unittest exécutera tous les tests dans un seul plan de test et vous donnera les résultats.

Enfin, si votre code source n'est pas à la racine du répertoire et contenu dans un sous-répertoire, par exemple dans un dossier appelé src/, vous pouvez indiquer à unittest où exécuter les tests pour qu'il puisse importer correctement les modules avec le paramètre -t :

    python -m unittest discover -s tests -t src
    
Dans le répertoir src/ unittest recherchera tous les fichiers test*.py  à l'intérieur du répertoire tests  et les exécutera.  
 
### Comprendre la sortie de test 

C'était un exemple très simple où tout passe, alors maintenant nous allons essayer un test qui échoue et interpréter la sortie.

sum() devrait pouvoir accepter d'autres listes de types numériques, comme les fractions.

En haut du fichier test.py , ajoutez une instruction d'importation pour importer le type Fraction  du module fractions  dans la bibliothèque standard :

```python
    from fractions import Fraction
```
 
Ajoutez maintenant un test avec une assertion qui attend la valeur incorrecte, dans ce cas que la somme de 1/4, 1/4 et 2/5 soit 1 :

```python
    import unittest
    
    from ma_somme import somme
    from fractions import Fraction

    class TestSum(unittest.TestCase):
      def test_list_int(self):
          """
          Tester qu'il peut additionner une liste d'entiers
          """
          donnee = [3, 2, 2]
          res = somme(donnee)
          self.assertEqual(res, 7)

      def test_list_fraction(self):
          """
          Tester qu'il peut additionner une liste de fractions
          """
          data = [Fraction(1, 4), Fraction(1, 4), Fraction(2, 5)]
          result = sum(data)
          self.assertEqual(result, 1)

    if __name__ == '__main__':
        unittest.main()
```

Si vous exécutez à nouveau les tests avec python -m unittest test, vous devriez avoir le résultat suivant :

      F.
    ======================================================================
    FAIL: test_list_fraction (test.TestSum)
    Tester qu'il peut additionner une liste de fractions
    ----------------------------------------------------------------------
    Traceback (most recent call last):
    File "/Users/gredey/PycharmProjects/testing/test.py", line 21, in test_list_fraction
    self.assertEqual(result, 1)
    AssertionError: Fraction(9, 10) != 1

----------------------------------------------------------------------
    Ran 2 tests in 0.001s

    FAILED (failures=1)
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 testing % 


Dans la sortie, vous verrez les informations suivantes :

La première ligne affiche les résultats d'exécution de tous les tests, un échoué ( **F**) et un réussi **( .)**

L' entrée de l'echec affiche quelques détails sur le test ayant échoué :

-  Le nom de la méthode de test ( test_list_fraction)
-  Le module de test ( test) et le cas de test ( TestSum)
-  Une trace jusqu'à la ligne défaillante
-  Le détail de l'assertion avec le résultat attendu ( 1) et le résultat réel ( Fraction(9, 10))
-  N'oubliez pas que vous pouvez ajouter des informations supplémentaires à la sortie du test en ajoutant le paramètre -v  à la commande python -m unittest.

  ### Exécuter vos tests depuis PyCharm    

Si vous utilisez l' IDE PyCharm , vous pouvez exécuter unittest ou pytest en suivant ces étapes :

Dans la fenêtre de l'outil Projet, sélectionnez le répertoire de tests .
Dans le menu contextuel, choisissez la commande d'exécution pour unittest. Par exemple, choisissez Exécuter 'Unitests in my Tests…' .
Cela exécutera unittest dans une fenêtre de test et vous donnera les résultats dans PyCharm

### Exécution de vos tests à partir du  Visual Studio code

Si vous utilisez l'IDE Microsoft Visual Studio Code, la prise en charge de unittest, nose et de pytest est intégrée au plug-in Python.

Si vous avez installé le plugin Python, vous pouvez configurer la configuration de vos tests en ouvrant la palette de commandes avec Ctrl+ Shift+P et en tapant « test Python ». Vous verrez une gamme d'options.

Choisissez Déboguer tous les tests unitaires et VSCode déclenchera alors une invite pour configurer le framework de test. Cliquez sur le rouage pour sélectionner le testeur ( unittest) et le répertoire personnel ( .).

Une fois cela configuré, vous verrez l'état de vos tests en bas de la fenêtre, et vous pourrez accéder rapidement aux journaux de test et relancer les tests en cliquant sur les icônes.

Cela montre les tests qui sont en cours d'exécution, mais que certains d'entre eux échouent.

## Tester des frameworks Web comme Django et Flask <a class="encre" id="frameworks"></a>

   Si vous écrivez des tests pour une application Web en utilisant l'un des frameworks populaires comme Django ou Flask, il existe des différences importantes dans la façon dont vous écrivez et exécutez les tests.
   
### Pourquoi elles sont différentes des autres applications <a class="encre" id="difference"></a>
      
   Pensez à tout le code que vous allez tester dans une application Web. Les itinéraires, les vues et les modèles nécessitent tous beaucoup d'importations et de connaissances sur les frameworks utilisés.

Ceci est similaire au test de la voiture au début de l'article : vous devez démarrer l'ordinateur de la voiture avant de pouvoir exécuter un test simple comme la vérification des feux.

Django et Flask vous facilitent la tâche en fournissant un framework de test basé sur unittest. Vous pouvez continuer à écrire des tests comme vous l'avez appris, mais les exécuter légèrement différemment.

### Comment utiliser l'exécuteur de test Django <a class="encre" id="executeur_django"></a>
   
   Le template startapp de Django  créé un fichier tests.py  dans le répertoire de votre application. Si vous ne l'avez pas déjà, vous pouvez le créer avec le contenu suivant :
   
 ```python
   
   from django.test import TestCase
   class MyTestCase(TestCase):
     # Your test methods
    
``` 

La principale différence avec les exemples jusqu'à présent est que vous devez hériter de django.test.TestCase au lieu de unittest.TestCase. Ces classes ont la même API, mais la classe TestCase de Django configure tous les états requis à tester.

Pour exécuter votre suite de tests, au lieu d'utiliser unittest en ligne de commande, vous utilisez manage.py test: 

    python manage.py test 
    
Si vous voulez plusieurs fichiers de test, remplacez  tests.py  par un dossier appelé tests, insérez un fichier vide à l'intérieur appelé __init__.py et créez vos fichiers test_*.py. Django les découvrira et les exécutera.

Plus d'informations sont disponibles sur le [site web de la documentation de Django](https://docs.djangoproject.com/en/2.1/topics/testing/overview/)

### Comment utiliser unittest et Flask  <a class="encre" id="unitest_flask"></a> 
  
 Flask nécessite que l'application soit importée puis définie en mode test. Vous pouvez instancier un client de test et utiliser le client de test pour envoyer des requêtes à n'importe quelle route de votre application.
 
 Toute l'instanciation du client de test est effectuée dans la méthode setUp de votre scénario de test. Dans l'exemple suivant, my_app est le nom de l'application.
 
 ```python
    import my_app
    import unittest


    class MyTestCase(unittest.TestCase):

      def setUp(self):
          my_app.app.testing = True
          self.app = my_app.app.test_client()

      def test_home(self):
          result = self.app.get('/')
          # Make your assertions
 
 ```
 
Vous pouvez ensuite exécuter les scénarios de test à l'aide de la commande python -m unittest discover .

Plus d'informations sont disponibles sur le [site Web de documentation de Flask](https://flask.palletsprojects.com/en/0.12.x/testing/) .

## Scénarios de test plus avancés <a class="encre" id="scenario"></a>

Avant de se lancer dans la création de tests pour votre application, souvenez-vous des trois étapes de base de chaque test :

   1.  Créez vos entrées
   2.  Exécuter le code, capturer la sortie
   3.  Comparer la sortie avec un résultat attendu
 
 Ce n'est pas toujours aussi simple que de créer une valeur statique pour l'entrée comme une chaîne ou un nombre. Parfois, votre application nécessitera une instance d'une classe ou d'un contexte. Que faites-vous alors?

Les données que vous créez en tant qu'entrée sont appelées **fixture** . Il est courant de créer des luminaires et de les réutiliser.

Si vous exécutez le même test et que vous transmettez des valeurs différentes à chaque fois et que vous attendez le même résultat, cela s'appelle la **paramétrisation**.

### Gestion des échecs attendus <a class="encre" id="echecs"></a>

Plus tôt, lorsque vous avez dressé une liste de scénarios à tester somme(), une question s'est posée : que se passe-t-il lorsque vous lui fournissez une mauvaise valeur, comme un seul entier ou une chaîne ?

Dans ce cas, vous vous attendez que somme() génère une erreur. Lorsqu'il génère une erreur, cela entraînerait l'échec du test.

Il existe une manière spéciale de gérer les erreurs attendues. Vous pouvez utiliser .assertRaises()en tant que gestionnaire de contexte, puis à l'intérieur du bloc "with" exécuter les étapes de test :

```python
import unittest

from ma_somme import somme


class TestSum(unittest.TestCase):
    def test_list_int(self):
        """
        Tester qu'il peut additionner une liste d'entiers
        """
        donnee = [3, 2, 2]
        res = somme(data)
        self.assertEqual(res, 7)

    def test_list_fraction(self):
        """
        Tester qu'il peut additionner une liste de fractions
        """
        data = [Fraction(1, 4), Fraction(1, 4), Fraction(2, 5)]
        result = sum(data)
        self.assertEqual(result, 1)

    def test_bad_type(self):
        data = "banana"
        with self.assertRaises(TypeError):
            result = somme(data)

if __name__ == '__main__':
    unittest.main()
```

  Ce cas de test ne réussira désormais que si somme(data) déclenche un TypeError. Vous pouvez remplacer TypeError par n'importe quel type d'exception de votre choix.
  
### Isoler les comportements dans votre application <a class="encre" id="isoler"></a>
  
Plus tôt dans l'article, nous avons appris ce qu'est un effet secondaire. Les effets secondaires rendent les tests unitaires plus difficiles car, chaque fois qu'un test est exécuté, il peut donner un résultat différent, ou pire encore, un test peut avoir un impact sur l'état de l'application et faire échouer un autre test !

Il existe quelques techniques simples que vous pouvez utiliser pour tester les parties de votre application qui ont de nombreux effets secondaires :

    -  Refonte du code pour suivre le principe de responsabilité unique 
    -  fair un mock de n'importe quel appel de méthode ou de fonction pour supprimer 
     les effets secondaires 
    -  Utilisation des tests d'intégration au lieu des tests unitaires pour cette partie 
     de l'application
     
Si vous n'êtes pas familier avec les mocks, consultez [ Python CLI Testing ](https://realpython.com/python-cli-testing/#mocks) pour quelques bons exemples.

### Rédaction de tests d'intégration <a class="encre" id="test_integration"></a>

Jusqu'à présent, vous avez principalement appris les tests unitaires. Les tests unitaires sont un excellent moyen de créer un code prévisible et stable. Mais en fin de compte, votre application doit fonctionner dès qu'elle démarre !

Les tests d'intégration consistent à tester plusieurs composants de l'application pour vérifier qu'ils fonctionnent ensemble. Les tests d'intégration peuvent nécessiter d'agir comme un consommateur ou un utilisateur de l'application en :

    -  Appel d'une API REST HTTP
    -  Appel d'une API Python
    -  Appeler un service Web
    -  Exécution d'une ligne de commande
    
Chacun de ces types de tests d'intégration peut être écrit de la même manière qu'un test unitaire, en suivant le modèle Input, Execute et Assert. La différence la plus significative est que les tests d'intégration vérifient plus de composants à la fois et auront donc plus d'effets secondaires qu'un test unitaire. De plus, les tests d'intégration nécessiteront la mise en place de plus de dispositifs, comme une base de données, une prise réseau ou un fichier de configuration.

C'est pourquoi il est important de séparer vos tests unitaires et vos tests d'intégration. La création de dispositifs requis pour une intégration telle qu'une base de données de test et les cas de test eux-mêmes prennent souvent beaucoup plus de temps à s'exécuter que les tests unitaires, vous pouvez donc ne vouloir exécuter les tests d'intégration qu'une seule fois avant de passer en production au lieu de le faire à chaque validation.

Un moyen simple de séparer les tests unitaires et d'intégration consiste simplement à les placer dans des dossiers différents :

      project/
            │
            ├── my_app/
            │   └── __init__.py
                │
                └── tests/
                    |
                    ├── unit/
                    |   ├── __init__.py
                    |   └── test_sum.py
                    |
                    └── integration/
                        ├── __init__.py
                        └── test_integration.py
                        
 Il existe de nombreuses façons d'exécuter uniquement un groupe sélectionné de tests. Le paramètre de spécification du répertoire source, -s, peut être ajouté au chemin des tests contenant unittest discover  :
 
     python -m unittest discover -s tests/integration
 
 unittest vous aura donné les résultats de tous les tests du répertoire tests/integration .
 
### Tester des applications basées sur les données <a class="encre" id="test_donnees"></a>

De nombreux tests d'intégration nécessiteront des données backend comme une base de données pour exister avec certaines valeurs. Par exemple, vous souhaiterez peut-être avoir un test qui vérifie que l'application s'affiche correctement avec plus de 100 clients dans la base de données, ou que la page de commande fonctionne même si les noms de produits sont affichés en japonais.

Ces types de tests d'intégration dépendront de différents dispositifs de test pour s'assurer qu'ils sont reproductibles et prévisibles.

Une bonne technique à utiliser consiste à stocker les données de test dans un dossier de votre dossier de test d'intégration appelé fixtures pour indiquer qu'il contient des données de test. Ensuite, dans vos tests, vous pouvez charger les données et exécuter le test.

Voici un exemple de cette structure si les données étaient constituées de fichiers JSON :

                project/
                      │
                      ├── my_app/
                      │   └── __init__.py
                      │
                      └── tests/
                          |
                          └── unit/
                          |   ├── __init__.py
                          |   └── test_sum.py
                          |
                          └── integration/
                              |
                              ├── fixtures/
                              |   ├── test_basic.json
                              |   └── test_complex.json
                              |
                              ├── __init__.py
                              └── test_integration.py
                              
 Dans votre scénario de test, vous pouvez utiliser la méthode setUp() pour charger les données de test à partir d'un fichier et exécuter de nombreux tests sur ces données de test. N'oubliez pas que vous pouvez avoir plusieurs cas de test dans un seul fichier Python, et unittest exécutera les deux. Vous pouvez avoir un scénario de test pour chaque ensemble de données de test :
 
 ```python
 
import unittest


class TestBasic(unittest.TestCase):
    def setUp(self):
        # Load test data
        self.app = App(database='fixtures/test_basic.json')

    def test_customer_count(self):
        self.assertEqual(len(self.app.customers), 100)

    def test_existence_of_customer(self):
        customer = self.app.get_customer(id=10)
        self.assertEqual(customer.name, "Org XYZ")
        self.assertEqual(customer.address, "10 Red Road, Reading")


class TestComplexData(unittest.TestCase):
    def setUp(self):
        # load test data
        self.app = App(database='fixtures/test_complex.json')

    def test_customer_count(self):
        self.assertEqual(len(self.app.customers), 10000)

    def test_existence_of_customer(self):
        customer = self.app.get_customer(id=9999)
        self.assertEqual(customer.name, u"バナナ")
        self.assertEqual(customer.address, "10 Red Road, Akihabara, Tokyo")

if __name__ == '__main__':
    unittest.main()
    
```

Si votre application dépend de données provenant d'un emplacement distant, comme une API distante , vous devez vous assurer que vos tests sont reproductibles. Si vos tests échouent parce que l'API est hors ligne ou s'il y a un problème de connectivité, cela pourrait ralentir le développement. Dans ces types de situations, il est préférable de stocker localement les données distants afin qu'elles puissent être accessible  à l'application.

La bibliothèque requests  propose un package gratuit appelé responses qui vous permet de créer des dispositifs de réponse et de les enregistrer dans vos dossiers de test. En savoir plus [sur leur page GitHub](https://github.com/getsentry/responses).

## Tests dans plusieurs environnements <a class="encre" id="environnement"></a>

Jusqu'à présent, vous avez testé une seule version de Python en utilisant un environnement virtuel avec un ensemble spécifique de dépendances. Vous voudrez peut-être vérifier que votre application fonctionne sur plusieurs versions de Python ou sur plusieurs versions d'un package. Tox est une application qui automatise les tests dans plusieurs environnements.

### Installation de Tox <a class="encre" id="tox"></a>

Tox est disponible sur PyPI sous forme de package à installer via pip:

        pip install tox
        
 Maintenant que Tox est installé, il doit être configuré.
 
### Configuration de Tox pour vos dépendances <a class="encre" id="config_tox"></a>

Tox est configuré via un fichier de configuration dans votre répertoire de projet. Le fichier de configuration Tox contient les éléments suivants :

        -  La commande à exécuter pour exécuter des tests
        -  Tous les packages supplémentaires requis avant l'exécution
        -  Les versions cibles de Python à tester

Au lieu d'avoir à apprendre la syntaxe de configuration de Tox, vous pouvez prendre une longueur d'avance en exécutant l'application de démarrage rapide :

          tox-quickstart
          
 L'outil de configuration Tox vous posera ces questions et créera un fichier tox.ini :
 
 ```Config file
[tox]
envlist = py27, py36

[testenv]
deps =

commands =
    python -m unittest discover
 ```
 
 Avant de pouvoir exécuter Tox, il faut que vous disposiez d'un fichier setup.py dans votre dossier d'application contenant les étapes d'installation de votre package. Si vous n'en avez pas, vous pouvez suivre ce [guide](https://packaging.python.org/en/latest/tutorials/packaging-projects/#setup-py) sur la façon de créer un setup.py avant de continuer.

Alternativement, si votre projet n'est pas destiné à être distribué sur PyPI, vous pouvez ignorer cette exigence en ajoutant la ligne suivante dans le fichier tox.ini sous l'en-tête [tox] :

          [tox]
          envlist = py27, py36
          skipsdist=True
          
Une fois que vous avez terminé cette étape, vous êtes prêt à exécuter les tests.

Vous pouvez maintenant exécuter Tox, et cela créera deux environnements virtuels : un pour Python 2.7 et un pour Python 3.6. Le répertoire Tox s'appelle .tox/. Dans le répertoire .tox/, Tox exécutera python -m unittest discover sur chaque environnement virtuel.

Vous pouvez exécuter ce processus en appelant Tox sur la ligne de commande :

              $ tox
              
Tox affichera les résultats de vos tests par rapport à chaque environnement. La première fois qu'il s'exécute, Tox prend un peu de temps pour créer les environnements virtuels, mais une fois qu'il l'a fait, la deuxième exécution sera beaucoup plus rapide.

### Exécuter Tox <a class="encre" id="executer_tox"></a>

La sortie de Tox est assez simple. Il crée un environnement pour chaque version, installe vos dépendances, puis exécute les commandes de test.

Il existe quelques options de ligne de commande supplémentaires qu'il est bon de retenir.

N'exécutez qu'un seul environnement, tel que Python 3.6 :

      tox -e py36
 
Recréez les environnements virtuels, au cas où vos dépendances auraient changé ou si les packages de site seraient corrompus :

      tox -r
      
 Exécutez Tox avec une sortie moins détaillée :
  
      tox -q
      
  Exécution de Tox avec une sortie plus détaillée :
   
       $ tox -v
       
Vous trouverez plus d'informations sur Tox sur le site Web de [documentation Tox](https://tox.wiki/en/latest/) .

## Automatiser l'exécution de vos tests <a class="encre" id="test_auto"></a>

Jusqu'à présent, vous avez exécuté les tests manuellement en exécutant une commande. Il existe des outils pour exécuter automatiquement des tests lorsque vous apportez des modifications et que vous les validez dans un référentiel de dépôt de source  tel que Git. Les outils de test automatisés sont souvent connus sous le nom d'outils CI/CD, qui signifie « Intégration continue/Déploiement continu ». Ils peuvent exécuter vos tests, compiler et publier toutes les applications, et même les déployer en production.

*Travis CI* est l'un des nombreux services CI ( intégration continue ) disponibles.

Travis CI fonctionne bien avec Python, et maintenant que vous avez créé tous ces tests, vous pouvez automatiser leur exécution dans le cloud ! Travis CI est gratuit pour tous les projets open-source sur GitHub et GitLab et est disponible moyennant des frais pour les projets privés.

Pour commencer, connectez-vous au site Web et authentifiez-vous avec vos identifiants GitHub ou GitLab. Créez ensuite un fichier appelé .travis.yml avec le contenu suivant :

```yaml
language: python
python:
  - "2.7"
  - "3.7"
install:
  - pip install -r requirements.txt
script:
  - python -m unittest discover
```
       
       
Cette configuration demande à Travis CI de :

       - Testez avec Python 2.7 et 3.7 (vous pouvez remplacer ces versions par celles 
        de votre choix.)
       -  Installez tous les packages qui sont listé dans requirements.txt(vous devez 
        supprimer cette section si vous n'avez aucune dépendance.)
       -  Exécuter python -m unittest discover pour exécuter les tests

Une fois que vous avez validé et pusher ce fichier, Travis CI exécutera ces commandes vers votre référentiel Git distant. Vous pouvez consulter les résultats sur leur site web.

## Et après <a class="encre" id="apres"></a>

Maintenant que vous avez appris à créer des tests, à les exécuter, à les inclure dans votre projet et même à les exécuter automatiquement, il existe quelques techniques avancées que vous pourriez trouver utiles à mesure que votre bibliothèque de tests se développe.

### Introduire des linters dans votre application <a class="encre" id="linter"></a>

Tox et Travis CI ont une configuration pour une commande de test. La commande de test que vous avez utilisée tout au long de ce article est *python -m unittest discover*.

Vous pouvez fournir une ou plusieurs commandes dans tous ces outils, et cette option est là pour vous permettre d'ajouter plus d'outils qui améliorent la qualité de votre application.

Un tel type d'application est appelé linter. Un linter regardera votre code et le commentera. Il pourrait vous donner des conseils sur les erreurs que vous avez commises, corriger les espaces de fin et même prédire les bogues que vous pourriez avoir introduits.

Pour plus d'informations sur les linters, lisez le [tutoriel Python Code Quality](https://realpython.com/python-code-quality/) .

* Peluchage passif avec flake8

Un linter populaire qui commente le style de votre code par rapport à la spécification [PEP 8 ](https://www.youtube.com/watch?v=Hwckt4J96dI) est flake8.

Vous pouvez installer flake8 en utilisant pip:

          pip install flake8
          
 Vous pouvez ensuite parcourir flake8 un seul fichier, un dossier ou un modèle :
 
          flake8 test.py
          test.py:6:1: E302 expected 2 blank lines, found 1
          test.py:23:1: E305 expected 2 blank lines after class or function definition, 
           found 1
          test.py:24:20: W292 no newline at end of file
          
Vous verrez une liste d'erreurs et d'avertissements pour votre code flake8 trouvé.

flake8 est configurable sur la ligne de commande ou dans un fichier de configuration de votre projet. Si vous souhaitez ignorer certaines règles, comme E305 indiqué ci-dessus, vous pouvez les définir dans la configuration. flake8 inspectera un fichier .flake8  dans le dossier du projet ou un fichier setup.cfg . Si vous avez décidé d'utiliser Tox, vous pouvez mettre la section de configuration de flake8   dans tox.ini.

Cet exemple ignore les répertoires .git et  __pycache__  ainsi que la règle E305 . En outre, il définit la longueur de ligne maximale sur 90 au lieu de 80 caractères. Vous constaterez probablement que la contrainte par défaut de 79 caractères pour la largeur de ligne est très restrictive pour les tests, car ils contiennent des noms de méthode longs, des littéraux de chaîne avec des valeurs de test et d'autres éléments de données qui peuvent être plus longs. Il est courant de définir la longueur de ligne pour les tests jusqu'à 120 caractères :

```yaml
[flake8]
ignore = E305
exclude = .git,__pycache__
max-line-length = 90
```
   
Vous pouvez également fournir ces options sur la ligne de commande :

        flake8 --ignore E305 --exclude .git,__pycache__ --max-line-length=90
        
 Une liste complète des options de configuration est disponible sur le [site Web de documentation](https://flake8.pycqa.org/en/latest/user/options.html) .

Vous pouvez maintenant ajouter flake8 à votre configuration CI. Pour Travis CI, cela ressemblerait à ceci :

```yaml
matrix:
  include:
    - python: "2.7"
      script: "flake8"
```

Travis lira la configuration .flake8 et échouera la construction si des erreurs de peluchage se produisent. Assurez-vous d'ajouter la dépendance flake8  à votre fichier requirements.txt .

* Peluchage agressif avec un formateur de code

flake8 est un linter passif : il recommande des modifications, mais vous devez aller modifier le code. Une approche plus agressive est un formateur de code. Les formateurs de code modifieront automatiquement votre code pour répondre à un ensemble de pratiques de style et de mise en page.

*black* est un formateur très impitoyable. Il n'a pas d'options de configuration et il a un style très spécifique. Cela en fait un outil idéal à mettre dans votre pipeline de test.

        Remarque : black nécessite Python 3.6+.
        
 Vous pouvez installer black via pip :
 
          pip install black
          
  Ensuite, pour exécuter black en ligne de commande, indiquez le fichier ou le répertoire que vous souhaitez formater :
  
          black test.py
          
### Garder votre code de test propre <a class="encre" id="test_propre"></a>

Lors de l'écriture de tests, vous constaterez peut-être que vous finissez par copier et coller beaucoup plus de code que vous ne le feriez dans des applications classiques. Les tests peuvent parfois être très répétitifs, mais ce n'est en aucun cas une raison pour laisser votre code bâclé et difficile à maintenir.

Au fil du temps, vous développerez beaucoup de [*technical debt*](https://martinfowler.com/bliki/TechnicalDebt.html) dans votre code de test, et si vous apportez des modifications importantes à votre application qui nécessitent des modifications de vos tests, cela peut être une tâche plus lourde que nécessaire en raison de la façon dont vous les avez structurés.

Essayez de suivre le principe **DRY** lors de la rédaction des tests : **D**on’t **R**epeat **Y**ourself.

Les fixtures et les fonctions de test sont un excellent moyen de produire un code de test plus facile à entretenir. Aussi, la lisibilité compte. Envisagez de déployer un outil de filtrage comme flake8 sur votre code de test :

        flake8 --max-line-length=120 tests/
        
### Test de la dégradation des performances entre les modifications <a class="encre" id="degradation"></a>

Il existe de nombreuses façons de comparer le code en Python. La bibliothèque standard fournit le module timeit , qui peut chronométrer les fonctions un certain nombre de fois et vous donner la distribution. Cet exemple exécutera 100 fois test() et affichera le résultat :

```python
def test():
    # ... your code

if __name__ == '__main__':
    import timeit
    print(timeit.timeit("test()", setup="from __main__ import test", number=100))
```

Une autre option, si vous avez décidé de l'utiliser comme un testeur pytest  , le plugin pytest-benchmark  sera utilisé. Cela fournit un dispositif pytest  appelé benchmark. Vous pouvez passer benchmark(), et il enregistrera la synchronisation dans les résultats de pytest.

Vous pouvez installer pytest-benchmark depuis PyPI en utilisant pip:

          pip install pytest-benchmark
          
 Ensuite, vous pouvez ajouter un test qui utilise le fixture et passe à exécution :
 
 ```python
 
 def test_my_function(benchmark):
 result = benchmark(test)
 
 ```
 
 L'exécution de pytest vous donnera des résultats de référence.
 Plus d'informations sont disponibles sur le [site Web de la documentation](https://pytest-benchmark.readthedocs.io/en/latest/) .
 
### Test des failles de sécurité dans votre application <a class="encre" id="securite"></a>

Un autre test que vous voudrez exécuter sur votre application consiste à vérifier les erreurs ou les vulnérabilités de sécurité courantes.

Vous pouvez installer *bandit* depuis PyPI en utilisant pip:

      pip install bandit
      
      Last login: Tue Jun 21 17:26:45 on console
(base) gredey@MacBook-Pro-de-OUEDRAOGO-2 ~ % pip3 install bandit
Collecting bandit
  Downloading bandit-1.7.4-py3-none-any.whl (118 kB)
     |████████████████████████████████| 118 kB 399 kB/s 
Requirement already satisfied: PyYAML>=5.3.1 in ./opt/anaconda3/lib/python3.9/site-packages (from bandit) (6.0)
Collecting stevedore>=1.20.0
  Downloading stevedore-3.5.0-py3-none-any.whl (49 kB)
     |████████████████████████████████| 49 kB 3.8 MB/s 
Collecting GitPython>=1.0.1
  Downloading GitPython-3.1.27-py3-none-any.whl (181 kB)
     |████████████████████████████████| 181 kB 157 kB/s 
Collecting gitdb<5,>=4.0.1
  Downloading gitdb-4.0.9-py3-none-any.whl (63 kB)
     |████████████████████████████████| 63 kB 1.1 MB/s 
Collecting smmap<6,>=3.0.1
  Downloading smmap-5.0.0-py3-none-any.whl (24 kB)
Collecting pbr!=2.1.0,>=2.0.0
  Downloading pbr-5.9.0-py2.py3-none-any.whl (112 kB)
     |████████████████████████████████| 112 kB 603 kB/s 
Installing collected packages: smmap, pbr, gitdb, stevedore, GitPython, bandit
Successfully installed GitPython-3.1.27 bandit-1.7.4 gitdb-4.0.9 pbr-5.9.0 smmap-5.0.0 stevedore-3.5.0
(base) gredey@MacBook-Pro-de-OUEDRAOGO-2 ~ % 
      
Vous pouvez ensuite passer le nom de votre module applicatif avec le paramètre -r , et cela vous donnera un résumé :

  (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 testing % bandit -r ma_somme
[main]	INFO	profile include tests: None
[main]	INFO	profile exclude tests: None
[main]	INFO	cli include tests: None
[main]	INFO	cli exclude tests: None
[main]	INFO	running on Python 3.9.7
Run started:2022-06-21 20:47:35.782311

Test results:
	No issues identified.

Code scanned:
	Total lines of code: 5
	Total lines skipped (#nosec): 0

Run metrics:
	Total issues (by severity):
		Undefined: 0
		Low: 0
		Medium: 0
		High: 0
	Total issues (by confidence):
		Undefined: 0
		Low: 0
		Medium: 0
		High: 0
Files skipped (0):
(base) gredey@MacBook-Pro-de-OUEDRAOGO-2 testing % 

Comme pour flake8, les règles bandit signalées sont configurables, et s'il y en a que vous souhaitez ignorer, vous pouvez ajouter la section suivante à votre fichier setup.cfg  avec les options :

            [bandit]
            exclude: /test
            tests: B101,B102,B301
            
 Plus de détails sont disponibles sur le [site Web GitHub](https://github.com/PyCQA/bandit) .
 
 
##  Conclusion <a class="encre" id="conclusion"></a>

Python a rendu les tests accessibles en intégrant les commandes et les bibliothèques dont vous avez besoin pour valider que vos applications fonctionnent comme prévu. Commencer à tester en Python n'a pas besoin d'être compliqué : vous pouvez utiliser unittest et écrire de petites méthodes maintenables pour valider votre code.

Au fur et à mesure que vous en apprendrez davantage sur les tests et que votre application se développera, vous pourrez envisager de passer à l'un des autres frameworks de test, comme pytest, et commencer à tirer parti de fonctionnalités plus avancées.


## Bibliographie <a class="encre" id="biblio"></a>

[https://docs.python.org/fr/3/library/unittest.html](https://docs.python.org/fr/3/library/unittest.html)
[https://docs.pytest.org/en/latest/](https://docs.pytest.org/en/latest/)
[https://docs.djangoproject.com/en/2.1/topics/testing/overview/](https://docs.djangoproject.com/en/2.1/topics/testing/overview/)
[https://flask.palletsprojects.com/en/0.12.x/testing/](https://flask.palletsprojects.com/en/0.12.x/testing/)
[https://realpython.com/python-cli-testing/#mocks](https://realpython.com/python-cli-testing/#mocks)
[https://flask.palletsprojects.com/en/0.12.x/testing/](https://flask.palletsprojects.com/en/0.12.x/testing/)
[https://github.com/getsentry/responses](https://github.com/getsentry/responses)
[https://packaging.python.org/en/latest/tutorials/packaging-projects/#setup-py](https://packaging.python.org/en/latest/tutorials/packaging-projects/#setup-py)
[https://tox.wiki/en/latest/](https://tox.wiki/en/latest/)
[https://realpython.com/python-code-quality/](https://realpython.com/python-code-quality/)
[https://www.youtube.com/watch?v=Hwckt4J96dI](https://www.youtube.com/watch?v=Hwckt4J96dI)
[https://flake8.pycqa.org/en/latest/user/options.html](https://flake8.pycqa.org/en/latest/user/options.html)
[https://martinfowler.com/bliki/TechnicalDebt.htm](https://martinfowler.com/bliki/TechnicalDebt.htm)
[https://pytest-benchmark.readthedocs.io/en/latest/](https://pytest-benchmark.readthedocs.io/en/latest/)
[https://github.com/PyCQA/bandit](https://github.com/PyCQA/bandit)


      


   