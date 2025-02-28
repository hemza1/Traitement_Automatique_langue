Manipulation de Corpus de Texte et Modélisation de Langage
Objectif du TP
L'objectif de ce TP est de manipuler des corpus de texte en les considérant comme des signaux sous forme de séquences de lettres. Le travail se divise en deux grandes parties :

Étude des liens entre la taille d'un corpus et la taille de son lexique, et vérification de la loi de Zipf sur la distribution des mots.
Application de modèles de signal sur des signaux discrets, avec une attention particulière à l'utilisation de chaînes de Markov pour la classification thématique de textes.
Environnement de travail
Vous avez le choix entre deux environnements de développement pour ce TP :

Environnement Unix avec Bash Shell (disponible par défaut dans les distributions Ubuntu).
Environnement Jupyter Notebook, avec un exemple fourni pour le TP1.
Les scripts sont principalement en Python, mais vous êtes libre d'utiliser d'autres langages. L'efficacité des algorithmes est un point essentiel, donc tenez compte de la complexité des opérations en fonction de la taille des corpus.

Ressources fournies
Les ressources nécessaires au TP sont contenues dans le fichier TP1_data.tgz qui comprend :

corpus_topic (2.9 Mo) : Corpus de dépêches de presse en texte brut, classées selon 4 thématiques : Culture, Économie, International, Sport.
frwiki-sample.txt.gz (56 Mo) : Extrait du dump (ancien) de Wikipedia en français.
orfeo_dump.txt.gz (20 Mo) : Corpus de transcription de conversations orales.
Début du TP : Tokenization
Le traitement initial consiste à appliquer une étape de tokenization, c'est-à-dire découper le flux de texte d'un corpus en une séquence de "tokens". Un programme Python (tokenize.py) est mis à votre disposition pour effectuer cette tâche en utilisant un algorithme glouton basé sur un lexique (lexique_code.txt) contenant 89,365 mots.

Les mots inconnus du lexique sont codés par le code 0.

Partie 1 : Loi de Zipf
Question 1 : Taille du Corpus et Taille du Lexique
Vous devez déterminer la relation entre la taille d'un corpus et la taille de son lexique. Pour cela, écrivez un programme qui calcule la taille du lexique pour différentes tailles de corpus. Les tailles de corpus à considérer sont : 100, 500, 1000, 1500, 2000, 4000, 5000, 10000, 20000, 30000, 40000, 50000, 80000, 100000, 200000, 500000, 1000000, 2000000, 5000000.

Exemple de sortie attendue :

python-repl
Copier
Modifier
100 63
500 201
1000 323
1500 406
...
5000000 60182
Question 2 : Comparaison de Distributions
Vous comparerez les distributions taille du corpus / taille du lexique pour les corpus frwiki-sample.txt et orfeo_dump.txt. Utilisez la commande Unix gnuplot pour afficher les courbes, en utilisant un fichier de données pour chaque corpus.

Exemple de commande :

bash
Copier
Modifier
set grid
set logscale x
set logscale y
set xlabel 'taille corpus'
set ylabel 'taille lexique'
plot 'wiki.data' w lp t 'écrit', 'orfeo.data' w lp t 'oral'
Si vous souhaitez sauvegarder la courbe dans un fichier PDF :

bash
Copier
Modifier
set terminal pdf
set output 'taille_lexique.pdf'
replot
Question 3 : Interprétation des Courbes
Analysez les courbes obtenues et discutez :

Du rapport entre la taille du corpus et la taille du lexique.
Des différences entre la langue orale (corpus orfeo_dump.txt) et la langue écrite (corpus frwiki-sample.txt).
Partie 2 : Vérification de la Loi de Zipf
Question 1 : Comparaison des Courbes
Créez un fichier contenant les mots classés par fréquence décroissante pour chaque corpus. Pour chaque corpus, générez une courbe rang / fréquence en utilisant gnuplot.

Comparez les courbes des corpus Wikipedia et Orfeo, et interprétez les résultats.

Question 2 : Comparaison entre Thématiques
Comparez les courbes des corpus des différentes thématiques présentes dans le répertoire corpus_topic (Culture, Économie, International, Sport). Que pouvez-vous déduire de ces comparaisons ?

Partie 3 : Chaînes de Markov pour la Classification Thématique
Question 1 : Calcul des Probabilités des Bigrammes
Écrivez un programme qui prend un corpus en entrée et calcule les probabilités des bigrammes (paires de mots consécutifs) dans le corpus, en utilisant la formule du maximum de vraisemblance :

𝑃
(
𝑊
2
∣
𝑊
1
)
=
∣
𝑊
1
,
𝑊
2
∣
∣
𝑊
1
∣
P(W2∣W1)= 
∣W1∣
∣W1,W2∣
​
 

Stockez ces probabilités dans un fichier JSON, qui servira de modèle pour chaque thème du corpus.

Question 2 : Calcul de la Log-Probabilité
Écrivez un programme qui prend en entrée un modèle de bigramme et un corpus tokenisé, et qui calcule la log-probabilité du corpus par rapport au modèle. Ignorez les bigrammes qui ne sont pas présents dans le modèle.

Question 3 : Classification Automatique des Thèmes
Utilisez le programme précédent pour classifier automatiquement le thème des fichiers de test. Pour chaque fichier test, calculez les valeurs de log-probabilité pour les 4 modèles thématiques (Culture, Économie, International, Sport). Attribuez le thème correspondant à la log-probabilité la plus petite.

Question 4 : Mesure du Taux de Bonne Classification
Mesurez le taux de bonne classification pour chaque thème en comparant les prédictions de vos modèles avec les étiquettes réelles des corpus de test. Analysez les résultats obtenus.

Question Bonus : Modèles Unigramme et Trigramme
Répétez les expériences précédentes en utilisant d'abord un modèle unigramme (probabilité d'un mot seul) puis un modèle trigramme (probabilité d'un mot donné les deux mots précédents). Comparez les résultats et discutez des avantages et inconvénients de chaque modèle.
