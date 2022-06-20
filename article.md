
### Table des matières

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

### Introduction  <a class="encre" id="intro"></a>
Les tests en Python sont un sujet énorme et peuvent avoir l'ère d' être très complexes à mettre en oeuvre, mais cela n'est pas le cas. Le programmeur python peut commencer à créer des tests simples pour son application en quelques étapes simples, puis les développer à partir de là.

Dans ce article, nous apprendrons à créer un test de base, à l'exécuter et à trouver les bogues avant nos utilisateurs ! nous découvriront les outils disponibles pour écrire et exécuter des tests, vérifier les performances de notre application et même rechercher des problèmes de sécurité.

1. **Tester votre code**  <a class="encre" id="test_code"></a>
    
   Il existe de nombreuses façons de tester un code en python.  Nous apprendrons les  
   techniques des étapes les plus élémentaires et travaillerons vers des méthodes avancées.
    
    1. **Tests automatisés ou manuels** <a class="encre" id="auto_manuel"></a>
    
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
    
   2. **Tests unitaires vs tests d'intégration** <a class="encre" id="unit_inte"></a>
    
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

   iii.  **Choisir un testeur** <a class="encre" id="choix_testeur"></a>

Il existe de nombreux testeurs disponibles pour Python. Celui intégré à la bibliothèque standard Python s'appelle unittest. Dans ce article, nous utiliserons des scénarios de test *unittest* et *unittest test runner*. Les principes de unittest sont facilement transférables à d'autres frameworks. Les trois test runner les plus populaires sont :

  * unittest
  * nose ou nose2
  * pytest
  
Il est important de choisir le meilleur testeur pour vos besoins et votre niveau d'expérience.

##### unittest
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

    Remarque : Soyez prudent si vous écrivez des cas de test qui doivent être exécutés à la fois en Python 2 et 3. 
    En Python 2.7 et versions antérieures, unittest s'appelle unittest2. 
     Si vous importez simplement depuis unittest,vous obtiendrez différentes versions avec différentes fonctionnalités entre Python 2 et 3.
     
 
 Pour plus d'informations sur unittest, vous pouvez explorer la [documentation de unittest](https://docs.python.org/3/library/unittest.html)
 
##### nose

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
 
##### pytest
 
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


2. Écrire votre premier test <a class="encre" id="premier_test"></a>

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
 
   * Où ecrire le test <a class="encre" id="ouecrire"></a>
   
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
    fonction __import__() intégrée. Au lieu de from ma_somme      import sum, vous pouvez écrire ce qui suit :
    
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
    
  
  * Comment structurer un test simple <a class="encre" id="structure"></a>

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

* Comment écrire des assertions

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

* Effets secondaires <a class="encre" id="Effets secondaires"></a>

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
  
  
 3. Exécution de votre premier test <a class = "encre" id="execution"></a>
 
  Maintenant que vous avez créé le premier test, vous souhaitez l'exécuter. Bien sûr, vous savez que cela va réussir, mais avant de créer des tests plus complexes, vous devez vérifier que vous pouvez exécuter les tests avec succès.
  
   * Exécution des testeurs
   
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

   * Comprendre la sortie de test <a class="encre", id="sortie_test"></a>
   
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
   
* Exécuter vos tests depuis PyCharm <a class="encre",  id="test_pycharme"></a>

Si vous utilisez l' IDE PyCharm , vous pouvez exécuter unittest ou pytest en suivant ces étapes :

Dans la fenêtre de l'outil Projet, sélectionnez le répertoire de tests .
Dans le menu contextuel, choisissez la commande d'exécution pour unittest. Par exemple, choisissez Exécuter 'Unitests in my Tests…' .
Cela exécutera unittest dans une fenêtre de test et vous donnera les résultats dans PyCharm

* Exécution de vos tests à partir du  Visual Studio code

Si vous utilisez l'IDE Microsoft Visual Studio Code, la prise en charge de unittest, nose et de pytest est intégrée au plug-in Python.

Si vous avez installé le plugin Python, vous pouvez configurer la configuration de vos tests en ouvrant la palette de commandes avec Ctrl+ Shift+P et en tapant « test Python ». Vous verrez une gamme d'options.

Choisissez Déboguer tous les tests unitaires et VSCode déclenchera alors une invite pour configurer le framework de test. Cliquez sur le rouage pour sélectionner le testeur ( unittest) et le répertoire personnel ( .).

Une fois cela configuré, vous verrez l'état de vos tests en bas de la fenêtre, et vous pourrez accéder rapidement aux journaux de test et relancer les tests en cliquant sur les icônes.

Cela montre les tests qui sont en cours d'exécution, mais que certains d'entre eux échouent.

4.  **Tester des frameworks Web comme Django et Flask** <a class="encre" id="frameworks"></a>

   Si vous écrivez des tests pour une application Web en utilisant l'un des frameworks populaires comme Django ou Flask, il existe des différences importantes dans la façon dont vous écrivez et exécutez les tests.
   
   **Pourquoi elles sont différentes des autres applications**
      
   Pensez à tout le code que vous allez tester dans une application Web. Les itinéraires, les vues et les modèles nécessitent tous beaucoup d'importations et de connaissances sur les frameworks utilisés.

Ceci est similaire au test de la voiture au début de l'article : vous devez démarrer l'ordinateur de la voiture avant de pouvoir exécuter un test simple comme la vérification des feux.

Django et Flask vous facilitent la tâche en fournissant un framework de test basé sur unittest. Vous pouvez continuer à écrire des tests comme vous l'avez appris, mais les exécuter légèrement différemment.

   *  **Comment utiliser l'exécuteur de test Django**
   
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

* **Comment utiliser unittest et Flask**
  
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

5. **Scénarios de test plus avancés** <a class="encre" id="scenario"></a>

Avant de se lancer dans la création de tests pour votre application, souvenez-vous des trois étapes de base de chaque test :

   1.  Créez vos entrées
   2.  Exécuter le code, capturer la sortie
   3.  Comparer la sortie avec un résultat attendu
 
 Ce n'est pas toujours aussi simple que de créer une valeur statique pour l'entrée comme une chaîne ou un nombre. Parfois, votre application nécessitera une instance d'une classe ou d'un contexte. Que faites-vous alors?

Les données que vous créez en tant qu'entrée sont appelées **fixture** . Il est courant de créer des luminaires et de les réutiliser.

Si vous exécutez le même test et que vous transmettez des valeurs différentes à chaque fois et que vous attendez le même résultat, cela s'appelle la **paramétrisation**.

* Gestion des échecs attendus

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
  
  * Isoler les comportements dans votre application