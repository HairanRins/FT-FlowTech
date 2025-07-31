# Architecture 

En matière de développement logiciel avec TypeScript, plusieurs architectures permettent de structurer le code de manière efficace, maintenable et évolutive. 
Le choix de l'architecture dépend souvent de la taille et de la complexité du projet, des besoins de l'équipe et des objectifs à long terme. 
Voici une présentation claire et concise des architectures les plus courantes, avec des exemples pour illustrer leur mise en œuvre.

-----

## Architecture Hexagonale (ou Ports & Adapters)

L'architecture hexagonale, également connue sous le nom d'architecture "Ports and Adapters", vise à isoler le cœur de l'application (le domaine métier) des détails techniques externes tels que les bases de données, les interfaces utilisateur ou les services tiers. 
L'idée est de permettre à l'application d'être pilotée indifféremment par des utilisateurs, des programmes, des tests automatisés ou des scripts batch.

**Principes clés :**

  * **Au centre, le domaine :** Le cœur de l'application contient la logique et les règles métier pures, sans aucune dépendance envers des frameworks ou des technologies externes.
  * **Les ports :** Ce sont des interfaces qui définissent comment le domaine interagit avec le monde extérieur. Il existe deux types de ports :
      * **Ports primaires (ou entrants) :** Ils sont l'API de l'application et sont appelés par les acteurs externes (ex: un contrôleur HTTP).
      * **Ports secondaires (ou sortants) :** Ils définissent les services dont le domaine a besoin, comme l'accès à une base de données ou l'envoi d'un email.
  * **Les adaptateurs :** Ce sont les implémentations concrètes des ports.
      * **Adaptateurs primaires :** Un contrôleur REST qui appelle un cas d'utilisation.
      * **Adaptateurs secondaires :** Un module d'accès à une base de données qui implémente l'interface du port de persistance.

**Exemple simplifié en TypeScript :**

Imaginons un service simple pour créer un utilisateur.

***Définition du Port et du Domaine (le cœur)***

```typescript
// src/domain/user.ts
export class User {
  constructor(public readonly id: string, public name: string, public email: string) {}
}

// src/domain/user.repository.ts (Port Secondaire)
export interface UserRepository {
  save(user: User): Promise<void>;
  findById(id: string): Promise<User | null>;
}

// src/domain/create-user.use-case.ts (Port Primaire implicite via l'appel)
export class CreateUser {
  constructor(private readonly userRepository: UserRepository) {}

  async execute(name: string, email: string): Promise<User> {
    const user = new User(Date.now().toString(), name, email);
    await this.userRepository.save(user);
    return user;
  }
}
```

***Implémentation de l'Adaptateur Secondaire (infrastructure)***

```typescript
// src/infrastructure/in-memory-user.repository.ts
import { User } from '../domain/user';
import { UserRepository } from '../domain/user.repository';

export class InMemoryUserRepository implements UserRepository {
  private users: User[] = [];

  async save(user: User): Promise<void> {
    this.users.push(user);
    console.log('User saved:', user);
  }

  async findById(id: string): Promise<User | null> {
    return this.users.find(user => user.id === id) || null;
  }
}
```

***Adaptateur Primaire (entrée de l'application)***

```typescript
// src/adapters/primary/user.controller.ts
import { CreateUser } from '../../domain/create-user.use-case';
import { InMemoryUserRepository } from '../../infrastructure/in-memory-user.repository';

const userRepository = new InMemoryUserRepository();
const createUser = new CreateUser(userRepository);

// Simulation d'un appel API
async function handleRequest() {
  const newUser = await createUser.execute('John Doe', 'john.doe@example.com');
  console.log('User created via controller:', newUser);
}

handleRequest();
```

-----

## Clean Architecture

Très similaire à l'architecture hexagonale, la "Clean Architecture" de Robert C. Martin (Uncle Bob) met également l'accent sur la séparation des préoccupations en organisant le logiciel en cercles concentriques. 
La règle principale est que les dépendances ne peuvent pointer que vers l'intérieur.

**Les couches :**

1.  **Entities (Entités) :** Au cœur, elles encapsulent la logique métier la plus générale.
2.  **Use Cases (Cas d'utilisation) :** Ils orchestrent le flux de données vers et depuis les entités. C'est ici que se trouve la logique applicative.
3.  **Interface Adapters (Adaptateurs d'interface) :** Cette couche convertit les données du format des cas d'utilisation et des entités vers le format le plus pratique pour les couches externes (ex: base de données, web).
4.  **Frameworks & Drivers (Frameworks et Pilotes) :** La couche la plus externe, où se trouvent les détails : l'interface utilisateur, la base de données, les frameworks web, etc.

**Exemple de structure de dossiers en TypeScript :**

```
src/
├── domain/
│   ├── entities/
│   │   └── user.entity.ts
│   └── repositories/
│       └── user.repository.interface.ts
├── application/
│   └── use-cases/
│       └── create-user.use-case.ts
├── infrastructure/
│   ├── persistence/
│   │   └── in-memory-user.repository.ts
│   └── http/
│       └── user.controller.ts
└── main.ts
```

L'implémentation du code est très proche de l'architecture hexagonale, avec une terminologie de couches légèrement différente.

-----

## Architecture en Oignon (Onion Architecture)

L'architecture en oignon, proposée par Jeffrey Palermo, est une autre variante des architectures précédentes. 
Elle renforce l'idée de dépendre d'abstractions plutôt que d'implémentations concrètes en organisant l'application en couches successives autour du domaine.

**Couches principales :**

  * **Domain Model (Modèle de Domaine) :** Au centre, contient les objets du domaine.
  * **Domain Services (Services du Domaine) :** Logique métier qui ne trouve pas sa place dans un objet du domaine.
  * **Application Services (Services Applicatifs) :** Correspondent aux cas d'utilisation et connectent l'interface utilisateur à la logique métier.
  * **Couche externe :** UI, Infrastructure, Tests.

La règle de dépendance est la même : les couches externes dépendent des couches internes.

-----

## Architecture en Microservices

L'architecture en microservices structure une application comme un ensemble de petits services autonomes. Chaque service est construit autour d'une capacité métier et peut être développé, déployé et mis à l'échelle indépendamment.

**Principes clés :**

  * **Haute cohésion, faible couplage :** Chaque service a une responsabilité unique et bien définie.
  * **Déploiement indépendant :** Une modification dans un service ne nécessite pas de redéployer toute l'application.
  * **Technologiquement agnostique :** Chaque service peut utiliser la pile technologique qui lui convient le mieux.
  * **Communication :** Les services communiquent entre eux via des mécanismes légers, souvent des API HTTP/REST, des WebSockets ou des files d'attente de messages.

**Exemple simplifié avec TypeScript (deux services) :**

***Service Utilisateurs (user-service)***

```typescript
// user-service/index.ts (avec Express.js)
import express from 'express';
const app = express();
const port = 3001;

const users = [{ id: 1, name: 'Jane Doe' }];

app.get('/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (user) {
    res.json(user);
  } else {
    res.status(404).send('User not found');
  }
});

app.listen(port, () => console.log(`User service listening on port ${port}`));
```

***Service Commandes (order-service)***

```typescript
// order-service/index.ts (avec Express.js)
import express from 'express';
import axios from 'axios';
const app = express();
const port = 3002;

app.get('/orders/user/:userId', async (req, res) => {
  try {
    // Appel à l'autre microservice pour obtenir les infos de l'utilisateur
    const userResponse = await axios.get(`http://localhost:3001/users/${req.params.userId}`);
    const userName = userResponse.data.name;

    // Logique de commande
    res.send(`Orders for user: ${userName}`);
  } catch (error) {
    res.status(500).send('Error fetching user data');
  }
});

app.listen(port, () => console.log(`Order service listening on port ${port}`));
```

-----

## Architecture Monolithique

C'est l'approche traditionnelle où l'ensemble de l'application est construit comme une seule et unique unité. Tous les composants (UI, logique métier, accès aux données) sont développés et déployés ensemble.

**Avantages :**

  * **Simplicité de développement au début :** Un seul projet à gérer.
  * **Déploiement simple :** Une seule application à déployer.
  * **Facilité de test de bout en bout.**

**Inconvénients :**

  * **Difficulté à maintenir** à mesure que l'application grossit.
  * **Mise à l'échelle complexe :** Il faut mettre à l'échelle l'ensemble de l'application, même si un seul composant est surchargé.
  * **L'adoption de nouvelles technologies est difficile.**

**Exemple de structure de dossiers en TypeScript pour un monolithe simple avec Express :**

```
src/
├── controllers/
│   └── user.controller.ts
├── services/
│   └── user.service.ts
├── repositories/
│   └── user.repository.ts
├── models/
│   └── user.model.ts
└── app.ts
```

Le code à l'intérieur de ces fichiers serait fortement couplé et toutes les fonctionnalités seraient gérées au sein du même processus Node.js.
