# Préparation entretien SOC

Règle : réponse orale en 1 minute max. Si j'hésite → je retravaille.

## Concepts fondamentaux (S1–2)
| # | Question | Ma réponse (courte) | Maîtrisé |
|---|---|---|---|
| 1 | Différence entre SIEM, EDR et SOAR ? | | ☐ |
| 2 | Quel est le rôle d'un analyste L1 ? | | ☐ |
| 3 | Comment fais-tu le triage d'une alerte ? | | ☐ |
| 4 | Faux positif vs vrai positif (vs bénin) ? | | ☐ |
| 5 | Quand et comment escalader ? | | ☐ |
| 6 | Sévérité vs priorité ? | | ☐ |
| 7 | Qu'est-ce qu'un IOC ? Exemples ? | | ☐ |
| 8 | Pourquoi documenter une investigation ? | | ☐ |
| 9 | Qu'est-ce qu'un playbook / workbook ? | | ☐ |

| 10 | Quelle est la responsabilité du L1 / workbook ? | *Le L1, c'est la première ligne : je traite la file d'alertes. Pour chaque alerte, je collecte les preuves dans le SIEM : qui, quelle machine, quelle IP, quand, et si c'est déjà arrivé. J'enrichis les indicateurs, puis je décide : faux positif, je clôture en justifiant ; vrai positif ou doute, j'escalade au L2 avec un ticket complet. Par exemple, sur une rafale d'échecs de connexion depuis une même IP, je vérifie s'il y a eu une connexion réussie juste après : c'est ce qui fait passer l'alerte de "tentative" à "compromission probable"* | X |

| 11 | Qu'est-ce qu'un MSSP  ? | Managed Security Services Provider : entreprise qui offre des services de sécurité en sous-traitances; la pluspart du temps des services SOC | ☐ |


| 9 | Le L1 a-t-il le droit d'agir lui-même, par exemple isoler une machine via l'EDR, ou est-ce réservé au L2 ou à l'IR ? | 
La réponse : ça dépend du SOC. Dans certaines organisations, le L1 peut lancer un confinement prévu par un playbook (via l'EDR ou le SOAR). Ailleurs, tout passe par le L2.| ☐ |

## Mises en situation
| # | Question | Ma réponse (courte) | Maîtrisé |
|---|---|---|---|
| 10 | 200 événements 4625 en 5 min depuis la même IP. Que fais-tu ? | | ☐ |
| 11 | Un utilisateur signale un mail suspect. Quelles sont tes étapes ? | | ☐ |
| 12 | PowerShell lancé avec `-enc` depuis Word. Que suspectes-tu ? | | ☐ |

<!-- Ajouter les nouvelles questions au fil des sections -->
