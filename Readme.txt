#Readme:  l'etrainement d'un systeme de traduction 

# seq2seq
Telecharger le Attention-based sequence to sequence learning
	wget https://github.com/eske/seq2seq/

il faut assurer que les compossant suivant sont bien installé
## Dependencies
* [TensorFlow for Python 3](https://www.tensorflow.org/versions/r0.11/get_started/os_setup.html)
* YAML and Matplotlib modules for Python 3: `sudo apt-get install python3-yaml python3-matplotlib`

## preparation des données d'entrainement: 
pour preparer les donner d'apprentissage il faut utiliser le scripte 'scripts/prepare-data.py'
ce scripte permet de créer le train le test le dev et le vocabulaire apartire d'un corpus paralelle et il posséde plusieur options :
exemple : 
scripts/prepare-data.py data/news fr en output --dev-corpus data/news-dev --no-tokenize --vocab-size 60000 --test-size 2000 --dev-size 2000 --lowercase --shuffle --subwords

# si vous voulez utiliser les subwords, apres avoir créé vocab.{l1,l2} et bpe.{l1,l2} faut utiliser le script 'scripts/concat-bpe.py' pour avoir le vocabulaire final, pour l'utiliser à l'entrainement 
exemple :  
scripts/concat-bpe.py vocab.fr bpe.fr > new_vocab.fr
mv new_vocab.fr vocab.fr

##Entrainer un modéle : 
le fichier de configuration utiliser par defaut est un fichier de type YAML se trouve dans `config/default.yaml`
il faut pas modifier directement sur ce fichier, de preference de faire une copie, sinon copier les parametres a modifier seulement dans un autre fichier( comme experiments/WMT14/baseline.yaml)

    python3 -m translate CONFIG --train -v

## Pour traduire un texte en utilisant un modele qui existe deja:

    python3 -m translate CONFIG --decode FILE_TO_TRANSLATE --output OUTPUT_FILE

## pour la traduction en utilisant plusieur model(ensemble) : 

python3 -m translate CONFIG --checkpoints [ model1 model2 ....]  --decode FILE_TO_TRANSLATE --output OUTPUT_FILE

exemple :
python3 -m translate experiments/NMT-Bpe/baselineDec.yaml --checkpoints experiments/NMT-Bpe/baseline/checkpoints/best-444000  experiments/NMT-Bpe/baseline/checkpoints/best-428000  experiments/NMT-Bpe/baseline/checkpoints/best-328000  experiments/NMT-Bpe-v2/baseline/checkpoints/best-580000 experiments/NMT-Bpe-v2/baseline/checkpoints/best-452000 experiments/NMT-Bpe-v2/baseline/checkpoints/best-404000 --decode INPUT.L1 --output OUTPUT.L2

* il faut ajouter dans CONFIG "ensemble: True" 


Example model:

    experiments/WMT14/download.sh    # download WMT14 data into data/raw
    experiments/WMT14/prepare.sh     # preprocess the data, and copy the files to experiments/WMT14/data
    python3 -m translate experiments/WMT14/baseline.yaml --train -v   # train a baseline model on this data


