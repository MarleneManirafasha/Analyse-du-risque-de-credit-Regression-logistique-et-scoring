## ANALYSE DE RISQUE DE CREDIT : CAS DU LOAN CLUB

### 0. Introduction

L’étude s’appuie sur un ensemble de 20 000 dossiers d’emprunteurs provenant d’une plateforme de crédit de type Loan Club. L’objectif principal est de construire un modèle de scoring permettant d’estimer la probabilité de défaut d’un client et d’identifier les facteurs influençant le risque.

Cette étude permets d'éclaire la prise de décision et éclairent notamment sur : 

- le seuil de score à utiliser pour accepter ou rejeter un dossier
- le risque réel associé aux déciles intermédiaires 
- les segments présentent le meilleur rapport risque/rentabilité ?
- le profil des bons et mauvais payeurs
- les durées ou types de prêts sont les plus risqués
- le gain réel en utilisant le score
- l'automatisation de certaines décisions

### 1. Les données
Le fichier loan_club.csv contient 20 000 observations et 26 variables. Les variables décrivent :

* ***Le prêt*** : montant (loan_amnt), durée (term), taux d’intérêt (int_rate), note de risque (grade), but du prêt (purpose)

* ***Profil emprunteur*** : ancienneté professionnelle (emp_length), revenu annuel (annual_inc), statut du logement (home_ownership)

* ***Historique de crédit*** : retards (delinq_2yrs), incidents publics (pub_rec), nombre de comptes ouverts (open_acc), etc.

* ***Variable cible***: loan_status (0 : remboursé, 1 : défaut)

### 2. Objectif de l'étude

L'étude vise à construire un modèle de scoring du risque de défaut. Plus précisément :

- analyser les variables influençant le défaut,

- créer un modèle prédictif (Logistic Regression),

- produire un score de probabilité de défaut,

- découper les clients en déciles de risque,

- analyser la cohérence du modèle.

En bref, il s'agit identifier qui risque de ne pas rembourser son crédit.


#### Concepts clés du scoring
* **Probabilité de défaut (PD)**
La PD mesure la probabilité qu’un emprunteur ne rembourse pas son crédit. Elle est comprise entre 0 et 1.

* **Score de crédit**
Le score représente une transformation de la PD utilisée pour classer les emprunteurs dans des catégories de risque.

* **Déciles de risque**
Classement des emprunteurs en 10 groupes du plus sûr au plus risqué. Un bon modèle montre une croissance monotone du taux de défaut d’un décile à l’autre.

* **Modèle de régression logistique**
Méthode statistique supervisée permettant d’estimer la probabilité d’un événement binaire. Elle fournit également une interprétation claire des variables influentes.

### 3. Méthodologie

La démarche adoptée est structurée en plusieurs étapes :
1. Importation et nettoyage des données ;
2. Préparation des variables (conversion, encodage) ;
3. Découpage des données en ensemble d'entraînement (train) et de test ;
4. Entraînement d’un modèle de régression logistique ;
5. Analyse de la performance (ROC, AUC, matrice de confusion) ;
6. Construction d’un score et segmentation par déciles ;
7. Visualisation de la cohérence entre probabilités prédites et taux de défaut observés.

#### Pourquoi un modèle logistique?

La régression logistique est hautement interprétable, ce qui en fait un choix privilégié dans les activités de crédit. La logistique se déploie facilement dans n’importe quel environnement, est rapide à entraîner et réentraîner, ne nécessite pas de ressources calculatoires importantes,

### 4. Les résultats principaux

- Le modèle présente une bonne aptitude discriminante (AUC supérieure à 0.65).
- Le taux de défaut augmente régulièrement d’un décile à l’autre, confirmant la cohérence du score.
- Les variables les plus déterminantes sont le taux d’intérêt, la durée du prêt, les enquêtes récentes, le revenu annuel et le grade.
- Les déciles 9 et 10 concentrent la majorité des emprunteurs à haut risque.

### 5. Perspectives d'évolution/amélioration

- Tester d'autres modèles avancés (Random Forest, XGBoost, Gradient Boosting).
- Ajouter des données comportementales ou temporelles pour enrichir le modèle.
- Améliorer la calibration des probabilités de défaut.
- Mettre en place un monitoring mensuel du score.
- Industrialiser le modèle dans un outil métier (API, dashboard).



