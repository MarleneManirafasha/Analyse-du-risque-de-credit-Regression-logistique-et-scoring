## ANALYSE DE RISQUE DE CREDIT : CAS DU LOAN CLUB

### **0. Introduction**

L’étude s’appuie sur un ensemble de 20 000 dossiers d’emprunteurs provenant d’une plateforme de crédit de type Loan Club. L’objectif principal est de construire un modèle de scoring permettant d’estimer la probabilité de défaut d’un client et d’identifier les facteurs influençant le risque.

Cette étude permets d'éclaire la prise de décision et éclairent notamment sur : 

- le seuil de score à utiliser pour accepter ou rejeter un dossier

- le risque réel associé aux déciles intermédiaires 

- les segments présentent le meilleur rapport risque/rentabilité ?

- le profil des bons et mauvais payeurs

- les durées ou types de prêts sont les plus risqués

- le gain réel en utilisant le score

- l'automatisation de certaines décisions

### **1. Les données**

Le fichier loan_club.csv contient 20 000 lignes et 26 variables regroupées en plusieurs catégories :

* ***Variables liées au prêt*** : montant (loan_amnt), durée (term), taux d’intérêt (int_rate), note de risque (grade), but du prêt (purpose)

* ***Variables socio-économiques*** : ancienneté professionnelle (emp_length), revenu annuel (annual_inc), statut du logement (home_ownership)

* ***Variables d’historique de crédit*** : retards (delinq_2yrs), incidents publics (pub_rec), nombre de comptes ouverts (open_acc), etc.

* ***Variable cible***: loan_status (0 : remboursé, 1 : défaut)

### **2. Objectif de l'étude**

L'étude vise à construire un modèle de scoring du risque de défaut. Plus précisément :

- analyser les variables influençant le défaut,

- créer un modèle prédictif (Logistic Regression),

- produire un score de probabilité de défaut,

- découper les clients en déciles de risque,

- analyser la cohérence du modèle.

En bref, il s'agit identifier qui risque de ne pas rembourser son crédit.


#### *Concepts clés du scoring*

* **Probabilité de défaut**
La probabilité de défaut mesure la probabilité qu’un emprunteur ne rembourse pas son crédit. Elle est comprise entre 0 et 1.

* **Score de crédit**
Le score représente une transformation de la probabilité de défaut utilisée pour classer les emprunteurs dans des catégories de risque.

* **Déciles de risque**
Classement des emprunteurs en 10 groupes du plus sûr au plus risqué. Un bon modèle montre une croissance monotone du taux de défaut d’un décile à l’autre.

* **Modèle de régression logistique**
Méthode statistique supervisée permettant d’estimer la probabilité d’un événement binaire (0 ou 1). Elle fournit également une interprétation claire des variables influentes.

### **3. Méthodologie**

La démarche adoptée est structurée en plusieurs étapes :

1. Nettoyage et préparation : Transformation des variables, encodage des données catégorielles, choix des références.

2. Séparation train / test : 75 % apprentissage — 25 % test, avec stratification.

3. Modélisation : Estimation d’un modèle logistique complet intégrant variables financières, variables socio-économiques et variables d’historique de crédit

4. Évaluation : AUC, matrice de confusion, recall, precision, comparaison taux réel vs. taux prédit par déciles

5. Construction du score et détermination du seuil optimal via l’indice de Youden.


#### *Pourquoi un modèle logistique?*

La régression logistique est hautement interprétable, ce qui en fait un choix privilégié dans les activités de crédit. La logistique se déploie facilement dans n’importe quel environnement, est rapide à entraîner et réentraîner, ne nécessite pas de ressources calculatoires importantes,

### **4. Les résultats principaux**

**a. Performance globale**

- AUC = 0.620. Le modèle discrimine correctement les bons et les mauvais payeurs.

- Le modèle classe bien les emprunteurs : Le taux de défaut augmente clairement du décile 1 au décile 10.

**b. Déciles de risque**

Les résultats montrent :

- Déciles 1 à 4 : taux de défaut entre 5 et 15 % -> emprunteurs très sûrs

- Déciles 5 à 7 : taux de défaut entre 15 % et 20 % -> risque intermédiaire

- Déciles 8 et 10 : taux de défaut entre 20 et 25 % -> profils à très haut risque

Cette structure permet de piloter les politiques d’octroi et de tarification.

**c. Variables les plus influentes (odds ratios les plus élevés)**

- Durée du prêt de 60 mois : augmente fortement le risque, avec un odds ratio de 1.266, soit un risque multiplié par 1.266 par rapport aux prêts de 36 mois.

- Le nombre de cas d'impayés en de 2 ans (*delinq_2yrs*) a un Odds Ratio de 1.055,c'est à dire qu'il augmente le risque de 5.5% pour chaque cas enregistrée au cours des deux dernières années.

- Statut *locataire* : augmente le risque de 7.6% par rapport aux autres statuts de propriété.

- Statut de propriété  "*Propriétaire*" (*home_ownership_proprietaire*) réduit la côte de défaut de 2.3% (1 - 0.977) par rapport à la catégorie de référence (Hypothèque ou Locataire avec hypothèque).

- Durée d'emploi : Senior (*emp_length_senior*) réduit le risque de 2.0%, traduisant une stabilité professionnelle bénéfique.

**d. Calibration**

Le modèle surestime la probabilité de défaut, parfois par un facteur deux à trois fois plus élevé. Il est donc :

- excellent pour classer les clients (segmentation),

- mais nécessite une calibration pour utiliser directement les probabilité de défaut en production.

### **5. Perspectives d'évolution/amélioration**

- Tester d'autres modèles avancés (Random Forest, XGBoost, Gradient Boosting).
- Ajouter des données comportementales ou temporelles pour enrichir le modèle.
- Améliorer la calibration des probabilités de défaut.
- Mettre en place un monitoring mensuel du score.
- Industrialiser le modèle dans un outil métier (API, dashboard).  

### **6. Conclusion**

Le modèle développé offre une excellente capacité de segmentation du risque, permettant d’identifier clairement les emprunteurs les plus sûrs et les plus risqués. Il constitue un outil fiable d’aide à la décision pour l’octroi, la tarification et le pilotage du portefeuille.

Même si les probabilités sont surestimées, la structure du score est robuste :
la montée du risque par décile est nette, cohérente et exploitable opérationnellement.

Ce travail constitue une base solide pour un système de scoring évolutif, pouvant être amélioré par des techniques de calibration et des modèles plus avancés.
