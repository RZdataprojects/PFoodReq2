This project is a reconstruction of the WSDM 2021 paper "Personalized Food Recommendation as Constrained Question Answering over a Large-scale Food Knowledge Graph" from https://arxiv.org/abs/2101.01775 and is based on the code provided by the writers at https://github.com/hugochan/PFoodReq.

In order to reproduce our expriements:

1. Clone the repository to a GPU supported environment.
2. Use pip install -r requirements.txt to install requirements
3. Then use pip install rapidfuzz and pip install torch==0.4.1 torchvision==0.2.1 -f https://download.pytorch.org/whl/torch_stable.html
4. Login via BGU to Google and download: https://drive.google.com/file/d/1hSIt-uoTvPOt9q7pIZKypkh9HLA_wCEZ/view?usp=sharing, place it in PFoodReq2\data\recipe_kg.
5. Go to the BAMnet/src folder

For each time you change embeddings, you need to re-process the data with your chosen embeddings:

Vectorize data


    python build_all_data.py -data_dir ../../data/kbqa_data/ -kb_path ../../data/recipe_kg/recipe_kg.json -out_dir ../data/kbqa


Fetch the pretrained vectors for our vocabulary.

    python build_pretrained_w2v.py -emb ../../data/glove.840B.300d.txt -data_dir ../data/kbqa/ -out ../data/kbqa/glove_pretrained_300d_w2v.npy

5. Then modify the config file BAMnet/src/config/kbqa.yml or BAMnet/src/config/pfoodreq_similar_recipes.yml. Set the correct directories for your server and change the embeddings size (i.e. 300 for GLOVE and 768 for GPT-2).

6. The default option is GLOVE embeddings and Cosine Similarity.

- To activate the use of GPT-2 embeddings, go to PFoodReq2/BAMnet/src/core/utils/generic_utils.py and in both load_embedding() methods, set "use_gpt_embeddings=True".
- To activate Manhatten Distance instead of Cosine Similiarity, go to BAMnet/src/core/recipe_similarity.py and in the get_cosine_distance() method set "type='manhatten'".

To train the KBQA model.

    python train.py -config config/pfoodreq.yml

To test the KBQA model:

    python run_online.py -config config/pfoodreq.yml

To train the KBQA+RecipeSim model.

    python train.py -config config/pfoodreq_similar_recipes.yml

To test the KBQA+RecipeSim model:

    python run_online.py -config config/pfoodreq_similar_recipes.yml
