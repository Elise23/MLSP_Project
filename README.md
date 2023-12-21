# MLSP_Project
MLSP Project about Speech Recognition

Présentation :
Notre but est de développer un pipeline capable de recevoir en entrée des audios contenant des erreurs grammaticales, de transformer ces audios en texte, corriger ces textes et reproduire un audio à partir du texte corrigé en sortie. L'idée est de permettre à un utilisateur d'apprendre une langue aussi bien à l'écrit qu'à l'oral, en visualisant ses erreurs écrites et en entendant la correction orale.

Fonctionnement :
Pour reproduire nos résultats, il suffit de s'assurer d'avoir toutes les dépendances d'installer dans son environnement, d'avoir tous les fichiers présents dans ce répertoire (les audios dans le dossier audio et les notebooks) et d'exécuter les cellules des notebooks. Les notebooks se lisent dans l'ordre suivant :
1. Dataset_Creation.ipynb
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
- pip install torchaudio
- pip install transformers
- pip install jiwer : pour utiliser la *Word Error Rate* (WER)
- téléchargement de libsndfile