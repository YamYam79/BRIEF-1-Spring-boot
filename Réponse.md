## 1. Qu'est-ce que Spring ?

C'est un framework open source. Il permet de créer des applications avec Java. 

Il permet de créer et de connecter des objets sans imposer d'implémenter des interfaces spécifiques.

Plus précisément, il permet de :
-gérer les dépendances de façon automatique en utilisant l'inversion de Contrôle (loC) et l'injection de Dépendances (DI)
-Structurer proprement une application puisqu'il permet de séparer la logique métier, l'accès aux données, la configuration...
-Créer des applications web via Spring MVC ou Spring WebFlux
-Accéder aux bases de données (ORM, JDBC, transactions)
-Intégrer d'autres technologies comme email, cache, JMS, tâches planifiées...
-Tester facilement au vu des outils intégrés
-sécuriser (Spring Security)

#Remarque "gérer les dépendances " :
Ca permet de ne plus créer directement les objets dont j'ai besoin.
C'est Spring qui donne directement au code ce qu'il a besoin et non l'inverse.

## Exemple :
Sans Spring, je dois écrire :
#Code
ServiceA serviceA = new ServiceA(new RepositoryA());

Obligation de construire RepositoryA, savoir comment construire ServiceA, gérer les dépendances et modifier le code si changement.
    
Avec Spring, j'écris juste :
#Code
@Service
public class ServiceA {
    private final RepositoryA repo;

    public ServiceA(RepositoryA repo) {
        this.repo = repo;
    }
}
Spring crée le Repositorya, le serviceA, injecte RepositoryA dans ServiceA et gère le cycle de vie des objets.

Ci-dessous les annotations qui permettent ces exécutions automatiques :
- @Service, @Repository,@Controller... => Spring détecte automatiquement les classes à instancier
- @Autowired => Spring comprend quelles dépendances fournir
- @Configuration + @Bean => Possibilité de définir manuellement des objets à créer


![[Pasted image 20260420120234.png]]
Parmi ces modules, il y a les modules fondamentaux qui font partie du noyau du Spring Framework (_core_) :

_Core_
Les classes fondamentales utilisées par tous les autre modules
_Beans_
Le module qui permet de manipuler les objets Java et de créer des _beans_
_Context_
Ce module introduit la notion de contexte d’application et fournit plusieurs implementations de ces contextes. Avec ce module, il est possible de construire un conteneur léger IoC.
_SpEL_
Ce module fournit un interpréteur pour le langage d’expression intégré au Spring Framework (_Spring Expression Language_ ou _SpEL_).
____
## 2. Quelle est la différence entre Spring et Spring Boot ?

Spring = framework, boîte à outils complète mais obligation avant de tout configurer soi-même (classes Java, serveurs, dépendances...).
Spring Boot = c'est une surcouche qui simplifie son utilisation.

Spring Boot ajoute :
 - l'Auto-configuration = il devine et configure automatiquement ce dont l'app a besoin.
    
- Le Serveur embarqué (Tomcat par exemple) = plus besoin d’installer un serveur externe.

- Les Dépendances font partie d'un package via starters (starter bbot, starter data...)
    
- Les commandes de lancement sont simples avec "run"
    
- Les Fichiers de configuration sont simplifiés (application.properties...)
    
- Actuator (qui est une dépendance) =>Il permet de monitorer et gérer l'application Spring Boot sans écrire les outils. 

____

## 3. Qu'est-ce que l'inversion de contrôle ?

L’**inversion de contrôle** (IoC, _Inversion of Control_) contrôle la création et la gestion des objets dans l'application.

Au lieu que le code crée et gère ses dépendances, c’est Spring qui s’en charge
## Pourquoi on appelle ça “inversion” ?

Parce que normalement, dans un programme classique, je contrôle tout :
Exemple :
#Code 
ServiceA service = new ServiceA(new RepositoryA());

Je décide quand RepositoryA, ServiceA et comment les connecter

Avec Spring, **le contrôle est inversé** :
Spring crée les objets, décide quand les instancier et les assemble entre eux

#Rq Avec Spring, le code ne fait que déclarer ce dont il a besoin

Spring applique l'inversion de contrôle grâce à 2 mécanismes :
			## Le conteneur LoC qui :
- scanne le code
- --détecte les classes-- annotées (@service, @ repository...)
- crée les objets correspondants (les beans)
- les stocke et les gère
			## L'injection de dépendances (ci-dessous)

___
## 4. Qu'est-ce que l'injection de dépendance
C'est la manière dont Spring _fournit les objets_ dont une classe à besoin.
#Code 
@Service
public class OrderService {
    private final PaymentService payment;

    public OrderService(PaymentService payment) {
        this.payment = payment;
    }
}
Je n'écris plus new PaymentService()
Spring voit que `OrderService` a besoin d’un  PaymentService = il l’injecte automatiquement.

Le code est ainsi plus modulaire (les classes n'ont pas de lien ensemble), plus testable, plus flexible (casse rien lors de changement d'implémentation), moins couplé (pas de dépendance avec New).