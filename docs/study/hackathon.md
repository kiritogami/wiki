# Modélisation et détection automatique

*Dernière mise à jour : {{ git_revision_date_localized }}*

## La Problématique

Je n'expliquerai pas pourquoi, mais durant ce hackathon, j'avais deux besoins :

- La première chose était de réaliser une **modélisation d'un skieur** ou, plus précisément, d'un capteur capable de recueillir certaines informations le concernant (principalement des données liées au **mouvement** et au **corps**).
- La deuxième était de déterminer, à partir de ces informations, si le mouvement était **normal ou anormal** (en gros, si le skieur était **en danger ou non**).

---

## 1. Simulation d'un skieur (capteur)

Pour quelqu'un qui n'a jamais pratiqué le ski ni la modélisation, c'était un vrai défi. Ma première question a été : **quelles informations dois-je simuler ?** J'ai alors identifié deux types d'informations : celles sur le mouvement et celles sur le corps (merci l'équipe fonctionnelle).

| Catégorie | Variables Simmulées |
| :--- | :--- |
| **Mouvement** | Position, Vitesse, Accélération, Attitude |
| **Corps** | Fréquence cardiaque, Consommation d'oxygène, Fréquence respiratoire, Température corporelle, Température cutanée |



Bien sûr, j'avais d'autres informations , mais par manque de temps, j'ai décidé de ne considérer que ces données-là (desolé équipe fonctionnelle :) ).

**Maintenant, la question cruciale : comment vais-je déterminer la manière dont toutes ces variables évoluent, sachant que je devais créer deux scénarios, un pour un skieur en sécurité et un autre où le skieur serait en danger ?**

Les idées que j'ai eues à ce moment :


- Un jeu vidéo de ski. 
- Les compétitions de ski.
- Utiliser la loi de Newton (merci ChatGPT).

Pour les gens qui detestent le mecanique, je vous conseille ignorer cette partie et rediriger vous directement vers la partie de scénario normal / anormal. 

### Loi de Newton :

La seconde loi de Newton est simple ; la somme des forces appliquées est égale au produit de la masse et de l'accélération. Dans notre cas, trois forces s'appliquent : la force de gravité, la résistance de la neige et la résistance de l'air.

$$F_g = m \cdot g \cdot \sin(\theta)$$
$$F_n = \mu \cdot N$$
$$F_a = \frac{1}{2} \cdot \rho \cdot C_D \cdot A \cdot v^2$$

$\mu$ represente le niveau de frottment entre les skis et la neige, une valeur eleve ca veut dire que le frotemmetn est fort c-a-d qu'on est dans une situation sûr.

$ C_D \cdot A$ mesure la surface que le corp oppose au vent., une valeur faible ca veut dire que la resistance de l'air est minimale


Sans détailler les calculs, pour un instant $t_{i+1}$ :

$$a_{i+1} = \frac{\mathbf{F}_{\text{sommes}}}{m} = \frac{\mathbf{F}_{\mathbf{g}} - (\mathbf{F}_{\mathbf{neige}} + \mathbf{F}_{\mathbf{air}})}{m}$$

$$\mathbf{v}_{i+1} = \mathbf{v}_i + \mathbf{a}_{i+1} \cdot \mathbf{\Delta t}$$

$$\mathbf{x}_{i+1} = \mathbf{x}_i + \mathbf{v}_{i+1} \cdot \mathbf{\Delta t}$$

Les valeurs initiales que je dois fournir sont :

| Type de Paramètre | Variable | Symbole | Définition | Valeur / Exemple |
| :--- | :--- | :--- | :--- | :--- |
| **Conditions Initiales** | Position initiale | $\mathbf{x_0}$ | Point de départ de la mesure de distance le long de la pente. | Généralement $\mathbf{0} \text{ m}$. |
| **Conditions Initiales** | Vitesse initiale | $\mathbf{v_0}$ | Vitesse du skieur au tout début de la simulation ($t=0$). | Une valeur choisie ($\ge 0 \text{ m/s}$), ex: $5 \text{ m/s}$. |
| **Constante Physique** | Masse du système | $m$ | Masse totale du skieur et de son équipement. | Ex: $80 \text{ kg}$. |
| **Constante Physique** | Gravité | $g$ | Accélération due à la pesanteur terrestre. | $\approx 9.81 \text{ m/s}^2$. |
| **Constante Physique** | Densité de l'air | $\rho$ | Masse de l'air par unité de volume. | $\approx 1.225 \text{ kg/m}^3$. |
| **Paramètre Piste** | Angle de la pente | $\mathbf{\theta}$ | Angle d'inclinaison de la piste par rapport à l'horizontale. | Ex: $15^\circ$. |
| **Paramètre Temporel**| Pas de temps | $\mathbf{\Delta t}$ | Intervalle de temps entre chaque calcul (itération) de la simulation. | Petit intervalle constant (ex: $0.1 \text{ s}$). |

### l'évolution des paramétres : 

le fonctionnement du modele est simple c'est une boucle qui se repetent à chaque intervalle de temps $\delta t$ et a chque iteration le code :
- determine les valeurs actuelles des parametre $ \mu $ et $ C_D \cdot A$
- Utilise la vitesse actuelle pour calculer $ \mathbf{F}_{\text{sommes}}$
- Calcule l'acceleration puis la nouvelle vitesse et la position.

La difference entre le sceneario normal et anormal reside dans la maniere comment les parametre $\mu$ et $ C_D \cdot A$ évoluent au cours de temps.

### scénario normal (mouvement) : 

je maintient $\mathbf{\mu}$ et $\mathbf{C_D \cdot A}$ à des valeurs moyennes , pour que la force de résistance ($\mathbf{F}_{\text{résistance}} = \mathbf{F}_{\mathbf{neige}} + \mathbf{F}_{\mathbf{air}})$ soit proche de la force de gravité $\mathbf{F}_{\mathbf{g}}$.
Par consequent laa $\mathbf{F}_{\text{somme}}$ reste faible, l'accélération est proche de zéro, et la vitesse se stabilise à un niveau constant et modéré.

### scénario anormal (mouvement) : 

À un instant prédéfini $\mathbf{t}_{\text{danger}}$ , Je code une chute brutale et soudaine de $\mathbf{\mu}$ et une augmentation de $\mathbf{C_D \cdot A}$. Donc La $\mathbf{F}_{\text{somme}}$ devient fortement positive, provoquant un pic d'accélération et une augmentation rapide et non maîtrisée de la vitesse.

d'une autre part pour les variables en relation avec le corps , j'ai choisie de modeliser 4 variables (FC,FR,temperature,VO2). l'idée principal c'etait de lier cela avec l'ffort physoiqe et au stress. 

a chaque seconde le skieur travaille pour contrer les forces et changer de direction : 
$$\mathbf{P}_{\text{effort}} = (\mathbf{F}_{\mathbf{n}} + \mathbf{F}_{\mathbf{a}}) \cdot \mathbf{v}$$

La consomation d'oxygene est proportionnel a cette puisance ? 

$$\mathbf{\dot{V}O_2} = \text{Constante}_{\text{effort}} \times \mathbf{P}_{\text{effort}} + \mathbf{\dot{V}O_2}_{\text{repos}}$$

Selon chatgpt O_O : 

$$\text{Constante}_{\text{effort}} = \mathbf{0,012 \text{ L d'O}_2 / (\text{min} \cdot \text{Watt})}$$

pour la temperature la cas simple c'était de la considérer comme constante.

**je n'avais pas le temps de comprendre comment evoluent FC et FR , donc j'ai ignoré les deux.**


---

## 2.Détection automatique : 

Puisque j'ai passé beaucoup de temps à comprendre comment je pouvais faire une simulation plus ou moins réaliste, je pense que vous l'avez remarqué, je n'avais pas beaucoup de temps pour travailler sur la partie de la détection.

En premier, ce que j'avais en tête, c'était d'ajouter un modèle de classification basé sur l'apprentissage automatique (je réfléchissais à des SVM puisque j'ai déjà travaillé avec ça). Le problème, c'est que je n'ai pas trouvé de données convenables pour mon cas.

Après réflexion et recherche, ce que j'ai décidé, c'est de plutôt créer un modèle qui répond à la question : « Est-ce que ce mouvement est normal ou non ? » Pourquoi ? Parce que j'ai déjà trouvé cette base de données : [kaggle data](https://www.kaggle.com/datasets/usmanabbasi2002/kfall-dataset=). Ainsi, je saurais au moins si le mouvement est anormal.

Pour les gens qui pensent qu'avec ça, j'aurais beaucoup de faux positifs, je suis d'accord avec vous. C'est pour ça que ce que je fais, c'est une vérification qui passe par 3 étapes :

- On vérifie si les variables de mouvement dépassent un seuil.

- Si la réponse est positive, on envoie ça à notre modèle pour qu'il nous dise si le mouvement est normal ou non .

- S'il dit que c'est anormal, c'est là qu'interviennent les variables du corps pour dire si ces variables sont normales ou non (NEWS SCORE).

Même avec ça, je dirais que j'aurais des faux positifs, mais enfin, notre objectif, c'est aussi de privilégier les faux positifs (un skieur qui est déclaré en danger mais en réalité il ne l'est pas, est moins impactant qu'un skieur qui est déclaré non en danger mais en réalité il est en danger).


## 3.Amélioration : 

Pour moi, au niveau de la simulation de mouvement, c'était plus ou moins réaliste. Par contre, il reste à ajouter les autres variables corporelles (FC et FR). Il reste aussi à mentionner que le capteur était simulé dans le back-end, ce qui est faux, car un capteur doit communiquer avec le back-end, c'est là qu'intervient le protocole avec lequel on va communiquer entre le back-end et le capteur (j'aimerais bien travailler sur ça dans le futur, c'est intéressant !). Pour le côté de la détection, il faut plus de travail sur la liaison des paramètres corporels et de mouvement. Il faut aussi voir comment prouver que ce que l'on fait génère des faux positifs mais pas de faux négatifs (en supposant qu'il le fait :) ).


## 4.conclusion : 

Pendant ces deux jours, j'ai pris de fausses décisions, certes. Pour certains, peut-être qu'ils penseront que pousser la simulation à ce degré là est inutile puisque c'est juste un hackathon, mais personnellement je voulais travailler sur ça (peut-être parce que ça fait longtemps que je n'ai pas fait de mécanique :)) mais l'idée, c'était de faire quelque chose de réaliste et d'ailleurs, c'est mon coéquipier qui m'a permis de faire cela, puisque lui s'occupait de toute la partie front-end. Donc,j'avais le temps pour travailler sur le backend et on voulait faire quelque chose de plus sophistiqué et plus réaliste (merci A.A.).

En dernier point, j'ai passé de bons moments et c'était une bonne expérience en général. Donc, ce que je peux dire en dernier point : **Merci StaySafe.**


**A faire : Je n'ai pas ajouté du code peut être que je vais le publier après, mais l'important c'est de voir comment le capteur peut communique avec le backend (hors ligne).**

