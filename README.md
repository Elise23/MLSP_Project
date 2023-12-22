# MLSP_Project
MLSP Project about Speech Recognition

## Présentation :

Notre but est de développer un pipeline capable de recevoir en entrée des audios contenant des erreurs grammaticales, de transformer ces audios en texte, corriger ces textes et reproduire un audio à partir du texte corrigé en sortie. L'idée est de permettre à un utilisateur d'apprendre une langue aussi bien à l'écrit qu'à l'oral, en visualisant ses erreurs écrites et en entendant la correction orale.

Fonctionnement :

Pour reproduire nos résultats, il suffit de s'assurer d'avoir toutes les dépendances d'installer dans son environnement, d'avoir tous les fichiers présents dans ce répertoire (les audios dans le dossier audio et les notebooks) et d'exécuter les cellules des notebooks. Les notebooks se lisent dans l'ordre suivant :
1. Dataset_Creation.ipynb (ce notebook a servi à créer les audios dans le fichier audios, il n'est donc pas nécessaire de réexécuter son contenu)
2. Speech_to_Text.ipynb
3. GEC.ipynb
4. TTS.ipynb
5. pipeline.ipynb

Concernant le modèle de *speech-to-text* vous pourrez le retrouver sur HuggingFace à ce nom : EliseB/whisper-small-dv. 

Project proposal : [Project_proposal_MLSP](Project_proposal_MSLP.pdf)

Dépendances :

- pip install gTTS datasets
- pip install librosa --user
- pip install import_ipynb
- pip install torchaudio deep_phonemizer
- pip install transformers datasets happytransformer
- pip install bert_score
- pip install jiwer : pour utiliser la *Word Error Rate* (WER)
- téléchargement de libsndfile



## Biais du pipeline
Nous allons étudier le biais du pipeline. Pour cela nous allons tester différentes phrases et erreurs pouvant survenir dans la pipeline.
Les exemples sont disponibles dans le dossier [exemples](./exemples/).

[ex_1](./exemples/ex_1/) = Dans un premier temps, nous allons tester la pipeline avec une phrase ne contenant pas de faute. Nous allons donc utiliser la phrase suivante prononcé de la meilleure façon possible : "I don't have a car, but I'm dreaming of it."
Nous obtenons la transcription suivante :
```text
i dont have a car but im dreaming of it
```
Nous pouvons observer que la phrase est bien juste. Néanmoins celle-ci ne contient pas de ponctuation, et les mots sont en minuscule. Passon maintenant à sa correction :
```text
I don't have a car, but I am dreaming of it.
```
On voit que la ponctuation a été corrigée, mais qu'il n'y a eu aucune correction au niveau des mots. Le modèle les a considérés comme corrects.
Concernant la synthèse vocale, nous obtenons le résultat suivant :
[ex_1.mp3](./exemples/ex_1/gtts_GEC/out_0.mp3)
___
[ex_2](./exemples/ex_2/) = En second temps, nous allons tester la pipeline avec une phrase contenant des fautes : "I dont have a car but I dreaming it off."
Nous obtenons la transcription suivante :
```text
i dont have a car but i dreaming it of
```
Nous pouvons observer que la phrase correspond bien à celle qui a été lue, seule différence le "off" compris comme "of". Passons à sa correction :
```text
I don't have a car, but I dream of it.
```
On voit que la ponctuation a été corrigée encore, Nous notons aussi que des modifications ont été faites en fin de phrase pour rendre la phrase juste. 
Concernant la synthèse vocale, nous obtenons le résultat suivant :
[ex_2.mp3](./exemples/ex_2/gtts_GEC/out_0.mp3)
___
[ex_3](./exemples/ex_3/) = Maintenant nous allons essayer de varier les erreurs dans la phrase : "I dont have a car but I am dreaming it off."
Nous obtenons la transcription suivante :
```text
i dont have a car but i am dreaming it up
```
Nous pouvons observer que la phrase n'a pas compris le "off", il as écrit en "up". Passons à sa correction :
```text
I don't have a car, but I am dreaming of it.
```
On voit que la correction a tout de même fonctionner, et nous a bien corriger la phrase. 
Concernant la synthèse vocale, nous obtenons le résultat suivant :
[ex_3.mp3](./exemples/ex_3/gtts_GEC/out_0.mp3)
___
[ex_4](./exemples/ex_4/) = Nous allons essayer d'enregistrer un audio sur fond musical (thème principal de Start Trek 🔥), et de voir comment le pipeline réagis : "I dont have a car but I dreaming it off."
Nous obtenons la transcription suivante :
```text
I dont have a car but i dream it
```
Nous pouvons observer que la phrase n'a pas compris la fin de l'enregistrement. Néanmoins il a réussi à extraire la majorité des mots, même avec du bruit musical en fond. Passons à sa correction :
```text
I don't have a car, but I dream of it.
```
On voit que la correction a tout de même fonctionner, et nous a bien corriger la phrase. 
Concernant la synthèse vocale, nous obtenons le résultat suivant :
[ex_4.mp3](./exemples/ex_4/gtts_GEC/out_0.mp3)
___
[ex_5](./exemples/ex_5/) = Essayons avec d'autres échantillons : "In the party last night everyone were dancing and having a good time but the music suddenly stops and nobody don't know why."
Nous obtenons la transcription suivante :
```text
In the party last night everyone were dancing and having a good time but the music suddenly stops and nobody don't know why.
```
On a une transcription ici parfaite de la phrase d'entrée. Passons à sa correction :
```text
In the party last night everyone was dancing and having a good time but the music suddenly stops and nobody don't know why.
```
On voit que la correction n'a rien modifié, quand bien même il y a une faute de grammaire ("nobody don't know why"). La phrase pourrait être corrigée en "nobody knew why" ce qui est formellement plus correct.
Concernant la synthèse vocale, nous obtenons le résultat suivant :
[ex_5.mp3](./exemples/ex_5/gtts_GEC/out_0.mp3)
___
[ex_6](./exemples/ex_6/) = Essayons avec encore un autre échantillon : "Last weekend me and my family goes on a road trip to the mountains but the car breaks down in the middle of nowhere and no one knows how to fixing it."
Nous obtenons la transcription suivante :
```text
Last weekend me and my family goes on a road trip to the mountains but the car breaks down in the middle of nowhere and no one knows how to fixing it.
```
On a une transcription encore ici parfaite de la phrase d'entrée. Passons à sa correction :
```text
Last weekend, me and my family went on a road trip to the mountains, but the car broke down in the middle of nowhere and no one knew how to fix it.
```
On voit que la correction a bien fonctionée, en prenant en compte le temps de la phrase et les ponctuations. 
Concernant la synthèse vocale, nous obtenons le résultat suivant :
[ex_6.mp3](./exemples/ex_6/gtts_GEC/out_0.mp3)

Conclusion:Néanmoins, il ne corrige pas toutes les fautes, et ne prend pas en compte les fautes de grammaire. Il est donc nécessaire de faire attention à ce que l'on dit, et de bien articuler pour que le pipeline fonctionne correctement.
