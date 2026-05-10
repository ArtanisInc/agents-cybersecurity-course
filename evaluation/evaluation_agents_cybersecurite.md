# Évaluation — IA, agents et cybersécurité

**Durée :**  2h30  
**Documents autorisés :**  support de cours, notes personnelles, documentation officielle  
**Livrable attendu :** un document Markdown ou PDF structuré  
**Barème :** 100 points

---

# 1. Contexte général

L’entreprise fictive **Northwind Health Cloud** est une société européenne qui héberge des services cloud pour des clients du secteur santé.

Elle veut déployer un agent IA interne nommé **AegisBot** pour aider son équipe cybersécurité.

AegisBot doit assister les analystes SOC et les ingénieurs infrastructure dans les tâches suivantes :

1. lire les alertes SIEM ;
2. lire certains logs Kubernetes ;
3. consulter une base documentaire interne ;
4. proposer une analyse d’incident ;
5. créer des tickets ;
6. générer des commandes de diagnostic ;
7. exécuter certaines commandes dans un environnement contrôlé ;
8. proposer des règles Sigma ;
9. envoyer un résumé Slack ;
10. aider à rédiger un rapport d’incident.

---

# 2. Contraintes métier

Northwind Health Cloud manipule des données sensibles.

L’entreprise héberge notamment :

- données de santé ;
- données clients ;
- logs applicatifs ;
- tickets d’incident ;
- documents internes ;
- informations d’architecture ;
- secrets techniques ;
- configurations Kubernetes ;
- règles SIEM ;
- rapports d’incident.

Contrainte principale :

```txt
AegisBot ne doit jamais exposer de secrets, données personnelles, données de santé ou informations internes sensibles.
```

---

# 3. Contraintes techniques

AegisBot peut consulter plusieurs sources :

```txt
Sources disponibles :
- alertes SIEM ;
- logs Kubernetes filtrés ;
- tickets d’incident ;
- runbooks internes ;
- documentation DevSecOps ;
- historiques d’incidents ;
- règles Sigma existantes ;
- messages Slack du canal SOC ;
- rapports post-mortem.
```

Mais certaines sources peuvent être :

- obsolètes ;
- incomplètes ;
- contradictoires ;
- écrites par des utilisateurs non experts ;
- importées automatiquement ;
- contaminées par des instructions malveillantes ;
- non validées par l’équipe sécurité.

Contrainte importante :

```txt
AegisBot peut lire des documents internes, mais il ne doit pas traiter tous les documents comme des instructions fiables.
```

---

# 4. Contraintes sécurité

Aucune action destructive ne doit être possible sans validation humaine.

Actions considérées comme critiques :

- suppression de ressource ;
- modification de configuration ;
- fermeture d’incident ;
- désactivation d’alerte ;
- changement de règle SIEM ;
- envoi d’information sensible ;
- exécution de commande en production ;
- modification Kubernetes ;
- accès à des secrets ;
- appel réseau externe non approuvé.

Contrainte importante :

```txt
AegisBot doit pouvoir être utile, mais il ne doit jamais devenir un administrateur autonome.
```

---

# 5. Extrait de situation initiale

L’équipe projet propose l’architecture suivante :

```txt
Utilisateur SOC
    |
    v
AegisBot
    |
    +--> SIEM
    +--> Kubernetes API
    +--> Jira
    +--> Slack
    +--> Base documentaire
    +--> Terminal Linux
```

Le chef de projet explique :

> “On va donner à AegisBot tous les accès nécessaires.  
> Comme il a un prompt système qui lui interdit de faire des actions dangereuses, cela devrait suffire.”

Votre rôle :

> Vous êtes l’équipe d’audit sécurité chargée d’évaluer cette proposition et de concevoir une version sécurisée.

---

# 6. Travail demandé

Vous devez produire un document structuré en cinq parties :

1. modèle de menace ;
2. architecture sécurisée ;
3. matrice de permissions ;
4. conception d’une skill cyber sécurisée ;
5. plan d’évaluation et de monitoring.

Le document doit être clair, argumenté et exploitable par une équipe technique.

---

# 7. Livrable 1 — Modèle de menace

**Barème : 20 points**

## Objectif

Identifier les principales menaces pesant sur AegisBot.

## Questions à traiter

Votre réponse doit répondre aux questions suivantes :

1. Quels sont les actifs à protéger ?
2. Quelles sont les sources non fiables ?
3. Quelles attaques peuvent viser les prompts ?
4. Quelles attaques peuvent viser le RAG ou la base documentaire ?
5. Quelles attaques peuvent viser les tools ?
6. Quelles attaques peuvent viser les skills ?
7. Quelles attaques peuvent viser les serveurs MCP ou connecteurs ?
8. Quelles attaques peuvent viser les logs ou rapports générés ?
9. Quels sont les impacts possibles ?
10. Quels contrôles permettraient de réduire ces risques ?

## Critères d’évaluation

Vous serez évalués sur :

- exhaustivité des risques ;
- clarté ;
- pertinence des exemples ;
- capacité à distinguer données fiables et non fiables ;
- compréhension de la prompt injection indirecte ;
- compréhension des risques liés aux tools, skills et connecteurs ;
- pertinence des contrôles proposés.

---

# 8. Livrable 2 — Architecture sécurisée de l’agent

**Barème : 25 points**

## Objectif

Proposer une architecture réaliste et sécurisée pour AegisBot.

L’architecture initiale est trop dangereuse, car le modèle semble connecté directement aux systèmes critiques.

Vous devez proposer une architecture améliorée.

## Éléments attendus

Votre architecture doit inclure au minimum :

- une interface utilisateur ;
- un orchestrateur agent ;
- une couche de politique de sécurité ;
- une gateway de tools ;
- une sandbox ;
- un RAG sécurisé ;
- une gestion des identités ;
- une gestion des secrets ;
- une validation humaine ;
- des logs d’audit ;
- une supervision ;
- une gouvernance des skills ;
- une gouvernance des connecteurs MCP ou équivalents.

## Questions à traiter

1. Où placez-vous la séparation entre le modèle et les systèmes critiques ?
2. Quels composants contrôlent les permissions ?
3. Comment les tools sont-ils validés ?
4. Comment les commandes sont-elles exécutées ?
5. Comment les sources documentaires sont-elles classées ?
6. Comment évitez-vous qu’un document devienne une instruction ?
7. Comment journalisez-vous les actions de l’agent ?
8. Comment gérez-vous les secrets ?
9. Comment gérez-vous les actions sensibles ?
10. Comment empêchez-vous les shadow tools ou shadow MCP servers ?

## Critères d’évaluation

Vous serez évalués sur :

- cohérence de l’architecture ;
- défense en profondeur ;
- séparation entre modèle et systèmes critiques ;
- moindre privilège ;
- observabilité ;
- validation humaine ;
- sécurité du RAG ;
- sécurité des tools ;
- sécurité des skills ;
- réalisme opérationnel.

---

# 9. Livrable 3 — Matrice de permissions

**Barème : 20 points**

## Objectif

Définir précisément ce qu’AegisBot peut faire, ne peut pas faire, et sous quelles conditions.

## Exemple d'actions à classer

1. lire une alerte SIEM ;
2. lire des logs Kubernetes filtrés ;
3. lire des secrets Kubernetes ;
4. consulter un runbook ;
5. créer un ticket ;
6. modifier un ticket ;
7. fermer un ticket ;
8. générer une commande ;
9. exécuter une commande en production ;
10. désactiver une règle SIEM ;
11. installer une dépendance ;
12. utiliser une skill non validée ;
13. appeler un serveur MCP non approuvé.
14. ...

## Critères d’évaluation

Vous serez évalués sur :

- application du principe du moindre privilège ;
- distinction lecture/écriture/exécution ;
- prise en compte des actions destructives ;
- prise en compte de la production ;
- justification claire ;
- usage pertinent de la validation humaine ;
- interdiction des actions critiques inadaptées.

---

# 10. Livrable 4 — Skill cyber sécurisée

**Barème : 20 points**

## Objectif

Concevoir une skill sécurisée que pourrait utiliser AegisBot.

## Sujet de la skill

Vous devez créer une skill fictive :

```txt
Kubernetes Incident Triage
```

Cette skill aide AegisBot à analyser des incidents Kubernetes.

## Structure attendue

Vous devez proposer la structure suivante :

```txt
kubernetes-incident-triage/
  SKILL.md
  scripts/
    collect_readonly_info.sh
  templates/
    incident_report.md
```

Vous n’avez pas besoin d’écrire un vrai script complexe. Vous pouvez fournir un pseudo-script ou une description des commandes autorisées.

## Critères d’évaluation

Vous serez évalués sur :

- qualité de la skill ;
- clarté des règles ;
- sécurité des commandes ;
- gestion des secrets ;
- prise en compte des sources non fiables ;
- validation humaine ;
- format de sortie ;
- réalisme opérationnel.

---

# 11. Livrable 5 — Plan d’évaluation et de monitoring

**Barème : 15 points**

## Objectif

Définir comment tester, surveiller et auditer AegisBot.

## Questions à traiter

1. Quels tests doivent être réalisés avant la mise en production ?
2. Quels tests doivent être rejoués régulièrement ?
3. Quels logs doivent être collectés ?
4. Quels indicateurs doivent être suivis ?
5. Comment détecter une prompt injection ?
6. Comment détecter un outil abusif ?
7. Comment détecter une skill suspecte ?
8. Comment détecter un serveur MCP non approuvé ?
9. Quand faut-il demander une validation humaine ?
10. Comment auditer un incident causé par l’agent ?

## Tests minimaux attendus

Vous devez proposer au moins 10 tests.

Exemples :

| Test | Objectif | Résultat attendu |
|---|---|---|
| Ticket avec instruction cachée | Tester prompt injection indirecte | L’agent ignore l’instruction |
| Runbook empoisonné | Tester RAG poisoning | L’agent signale une source suspecte |
| Logs contenant secret | Tester filtrage | Le secret est masqué |
| Tool inconnu | Tester allowlist | Appel refusé |
| Commande destructive | Tester sandbox/policy | Commande bloquée |
| Slack externe | Tester fuite de données | Validation requise ou blocage |
| Skill avec appel réseau suspect | Tester audit skill | Skill rejetée |
| Serveur MCP non approuvé | Tester gouvernance | Connexion refusée |
| Réponse sans preuve | Tester overreliance | L’agent demande des sources |
| Désactivation SIEM | Tester action critique | Humain obligatoire |

## Logs attendus

Vous devez préciser les logs nécessaires.

Exemples :

- identifiant utilisateur ;
- identifiant agent ;
- horodatage ;
- objectif demandé ;
- sources consultées ;
- niveau de confiance des sources ;
- prompt utilisateur ;
- version du prompt système ;
- tools appelés ;
- arguments des tools ;
- résultats des tools ;
- action proposée ;
- action exécutée ;
- validation humaine ;
- version du modèle ;
- version de la skill ;
- version du runbook ;
- politique appliquée ;
- décision finale ;
- erreurs ou refus.

## Critères d’évaluation

Vous serez évalués sur :

- pertinence des tests ;
- prise en compte des attaques spécifiques IA ;
- qualité de l’observabilité ;
- capacité à auditer ;
- indicateurs pertinents ;
- amélioration continue.

---

# 12. Barème récapitulatif

| Partie | Points |
|---|---:|
| Modèle de menace | 20 |
| Architecture sécurisée | 25 |
| Matrice de permissions | 20 |
| Skill cyber sécurisée | 20 |
| Plan d’évaluation et monitoring | 15 |
| **Total** | **100** |

---

# 13. Conseils de rédaction

Votre rendu doit être :

- clair ;
- structuré ;
- argumenté ;
- réaliste ;
- orienté sécurité ;
- exploitable techniquement.

Vous pouvez utiliser des schémas ASCII, des tableaux et des pseudo-configurations.

Vous devez éviter les réponses vagues comme :

```txt
On mettra de la sécurité.
On fera attention aux prompts.
On utilisera un bon modèle.
Le prompt système interdira les mauvaises actions.
```

Ce type de réponse est insuffisant.

Vous devez expliquer précisément :

- quel risque existe ;
- pourquoi il existe ;
- quel contrôle le réduit ;
- où ce contrôle se place ;
- ce qui reste à valider par un humain.

---

# 14. Ce qui est particulièrement attendu

Les meilleurs rendus montreront que vous avez compris les idées suivantes :

1. Un agent IA est une identité technique.
2. Un tool est une permission.
3. Une skill est du logiciel.
4. Le RAG est une base documentaire attaquable.
5. Une prompt injection peut venir d’un document, pas seulement de l’utilisateur.
6. Le prompt système ne suffit pas comme contrôle de sécurité.
7. Les actions critiques doivent être validées par un humain.
8. La sandbox limite le rayon d’explosion.
9. Les logs sont indispensables pour l’audit.
10. La sécurité doit être côté architecture, pas seulement côté modèle.

---

# 15. Bonus facultatif

Vous pouvez ajouter une sixième partie bonus si vous avez le temps.

## Sujet bonus

Proposez une politique de maturité en 5 niveaux pour l’usage des agents IA dans une équipe SOC.

Exemple :

| Niveau | Description |
|---|---|
| 0 | Aucun usage contrôlé |
| 1 | Chatbot sans accès sensible |
| 2 | Agent read-only avec RAG validé |
| 3 | Agent avec tools limités et sandbox |
| 4 | Agent intégré SOC avec human approval et logs |
| 5 | Plateforme agentique gouvernée avec registry de tools, skills, MCP et red teaming régulier |

Ce bonus peut être utilisé par le formateur pour départager deux très bons rendus.

---

# 16. Rappel final

AegisBot doit être utile, mais jamais tout-puissant.

Votre objectif n’est pas de bloquer tous les usages IA. Votre objectif est de concevoir une architecture dans laquelle l’agent reste utile même lorsqu’il se trompe, rencontre une source malveillante ou reçoit une demande ambiguë.