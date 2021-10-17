# Adversarial-Attacks-on-AlBERT

Create a new conda environment and install pytorch as per your requirements. I installed it using the command below:
    • conda install pytorch torchvision cudatoolkit=9.2 -c pytorch
    • pip install transformers
    • pip install pytorch-transformers
    • conda install nltk
    
Running the code:
The main file run is 'main.py'. It does not require any command-line arguments however there are two variables on top of the file that need to be set:
    • **squad_dataset_filepath**:	Set this filepath, if the adversarial attack is to be performed on the SQuAD dataset (determined by the next variable). Please note that AddAny is a very computation intensive technique hence it will take forever to run it on the complete squad dataset. You can either modify the extracted questions at line 57.
    • **use_squad_dataset**: 	If True, squad dataset file placed at above mentioned path will be used, otherwise the provided example of question/answers will be used for adversarial attacks.
    
Additionally, at the top of Adverserial.py there are five arguments that can be specified:
    • NUM_EPOCHS: Number of epochs to be run in each Mega Epoch.
    • NUM_SAMPLE: Number of alternative words to be tried in each iteration.
    • NUM_ADDITIONS: Sequence length of the sentence to be added.
    • NUM_SEARCHES_PER_MEGA_EPOCH: 

The goal in AddAny technique is to choose any sequence of d=10 words {w1, …., wd} that when added at the end of question context, minimizes the expected F1 score of the model for a given question. We first initialize words {w1, …., wd} randomly from 1000 most frequent words in brown corpus of words in nltk and the words contained in question.
Then we run 3 epochs of local search. In each epoch we iterate over all the words in the sequence {w1, …., wd} in a random order (adversarial.py line 207-209). 
The word wi that is being iterated over, is replaced by a set of candidate words to make multiple candidate sentences (adversarial.py-function generate_candidates). 

The candidate words are generated as a union of randomly sampled 20 words from nltk brown corpus and the words in the question. 
All the candidate words are replaced in place of wi one-by-one and the model is queried for the resulting question. (adversarial.py-function store_candidates) 

If all the candidate words perform poorer than the original word, the original word is kept as is. Otherwise, wi is replaced by the candidate word that helped achieve the minimum score. (adversarial.py- lines 221-234) 
