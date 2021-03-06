# UnitTESTSymfony

Home /Symfony 4 – Comment faire des tests unitaires

Symfony 4 – Comment faire des tests unitaires
L’intérêt de faire des tests unitaires, c’est de pouvoir tester son application pendant son développement. Cet article cible les développeurs web qui n’ont peu ou jamais fait de test unitaire sur un projet Symfony 4 ou 3.4 Flex.

Qu’est-ce qu’un test unitaire ?
Un test unitaire est une procédure qui permet de tester les fonctions dans des entités, des services etc. Afin de vérifier le bon fonctionnement de ce dernier.

Cependant, les tests sont faits à la charge du développeur pendant son développement en testant lui-même les fonctionnalités. Ce qui peut être amené à faire de la régression.

Cela permet aussi d’établir des règles métiers spécifique pour valider à chaque fois votre application avant chaque déploiement en production.

Pour créer un test unitaire sur un projet Symfony, on va utiliser PhpUnit.

Qu’est-ce que PhpUnit ?
PhpUnit est un framework PHP qui permet de faire des tests d’assertions. A savoir qu’un test d’assertion est une expression qui doit être évaluée vrai. PhpUnit est le plus utiliser et recommander dans plusieurs frameworks (Symfony, Laravel, Zend … ).

Comment ça marche ?
Dans un premier temps, il faut l’installer sur notre projet Symfony :

composer require --dev symfony/phpunit-bridge
Désormais dans votre dossier bin, vous avez le binaire phpunit.

L’ensemble de nos tests seront écrites dans le dossier tests qui est prévus à cette effet.

Pour la configuration de PhpUnit, il y a un fichier à la racine du projet nommé : phpunit.xml.dist

Test sur une entité
Passons désormais à un exemple, on souhaite tester l’entité Article de notre projet Blog.

```
// src/Entity/Article.php
namespace App/Entity;
class Article
{
    private $uri;
    private $title;
    public function setUri(string $uri)
    {
        $this->uri = strtolower(str*replace(' ', '*', $uri));
        return $this;
    }

    public function getUri()
    {
        return $this->uri;
    }
    public function setTitle(string $title)
    {
        $this->title = $title;
        return $this;
    }
    public function getTitle()
    {
        return $this->title;
    }

}
```

Pour des tests unitaires, il n’est pas important de tester des champs non modifiés dans une entité.

Maintenant je vais tester notre entité Article. Pour cela, je vais créer dans le dossier tests, le fichier ArticleTest.

Conseil de bonne pratique, le dossier tests doit avoir la même architecture que le dossier src pour faciliter la compréhension.

Exemple : src/Entity/Article.php son fichier de tests sera ici : tests/Entity/ArticleTest.php

```namespace App\Tests\Entity;
use App\Entity\Article;
use PHPUnit\Framework\TestCase;
class ArticleTest extends TestCase
{
    public function testUri()
    {
        $article = new Article();
        $uri = "Test 2";

        $article->setUri($uri);
        $this->assertEquals("test_2", $article->getUri());
    }
}
```

Test sur un service
Autre exemple de test unitaire, je vais créer un calculateur qui va uniquement retourner un résultat simple :

```
// src/Utils/Calculator.php
namespace App\Utils;
class Calculator
{
    public function add($a, $b)
    {
        return $a + $b;
    }
}
```

Passons désormais à un exemple, on souhaite tester un service de notre projet Blog.

Pour tester ce calculateur, je vais créer le fichier test correspondant.

```
// tests/Utils/CalculatorTest.php
namespace App\Tests\Utils;
use App\Utils\Calculator;
use PHPUnit\Framework\TestCase;
class CalculatorTest extends TestCase
{
    public function testAdd()
    {
        $calculator = new Calculator();
        $result = $calculator->add(10, 32);

        $this->assertEquals(42, $result);
    }
}
```

Pour lancer le test, il y a plusieurs façons de faire, soit l’ensemble des tests, soit dans un dossier spécifique ou un fichier de test spécifique.

# Lance l'ensemble des tests

\$ php bin/phpunit

# Lance l'ensemble des tests dans le dossier Entity.

\$ php bin/phpunit tests/Entity

# Lance l'ensemble des tests dans le fichier ArticleTest.php

\$ php bin/phpunit tests/Entity/ArticleTest.php
Conclusion
Désormais, vous pouvez créer des tests unitaires afin de tester votre application web. Un conseil, prioriser les tests sur les fonctionnalités métier.
