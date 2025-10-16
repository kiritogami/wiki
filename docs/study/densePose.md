# Detecter des humains avec les signaux WIFI

En juillet dernier, j'ai vu sur X un tweet intéressant qui parlait d'une feuille de recherche sortie et qui traitait le sujet de la détection de la pose des personnes à partir d'un signal Wi-Fi. Bien que l'idée semble fascinante au premier abord, l'appliquer reste compliquée.

Dans ce qui suit, j'expliquerai ce que j'ai compris jusqu'à ce moment-là en lisant cette feuille.
Situation initiale : Dans une pièce, on dépose deux routeurs Wi-Fi et un humain.

### Chaneel state information

Ou bien, le CSI, c'est un rapport entre le signal que nous avons émis et ce que nous avons reçu.  

le signal CSI est composée de 2 choses : 
- l'amplitude.  
- la phase (riche d'information sur la pose humaine).  

un tenseur : un tableau avec 3 dimension ou plus.
Le signal CSI est mesuré pour 3 émetteurs, 3 récepteurs, et 5 échantillons consécutifs sur 30 fréquences différentes.
ce que nous aurons en entree ce sont 2 tenseurs (tableau de 150*3*3) un pour l'amplitude et l'autre pour la phase.

### Etape 1 : Nettoyage du signal 

### Etape 2 : Modality Translation Network 

Les deux informations (l'amplitude et la phase) sont séparées dans cette étape. On essaie d'aplatir chaque tenseur, puis de faire un certain encodage et, enfin, une fusion pour avoir un long vecteur 1D.

Après, on fait un reshape, ensuite une extraction spatiale et on génère notre fausse image.

Le résultat est une fausse image qui sera regardée par un autre modèle dans l'étape 3.

### Etape 3 : Detection de Pose

À partir de l'image, et à l'aide d'un réseau de neurones, on obtient notre pose.

## Questions ?
- Les routeurs sont-ils des routeurs normaux ?
- Comment le CSI est-il transféré ?
- La prochaine fois, concentre-toi plus sur le vecteur 1D que nous avons eu en étape 2.