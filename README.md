# BLIP2-Japanese

This project builds upon [LAVIS](https://github.com/salesforce/LAVIS) library's BLIP2 mdoel.

The main idea is to replace the tokenizer and the underlying BERT model in Blip2's Qformer with the one trained on Japanese datasets and retrain the upated model on Japanese captioning datasets.

The model has been trained using COCO dataset with [STAIR captions](http://captions.stair.center/#:~:text=STAIR%20Captions%20is%20a%20large,multimodal%20retrieval%2C%20and%20image%20generation.).

## Quick Start

The weights of Blip2_Japanese_qformer trained on STAIR can be obtained from [this link](https://drive.google.com/drive/folders/11YRyQb-_Pn8g3Wlnv2aBwNnvZ0Oo4LRM?usp=drive_link).

Copy the whole folder under lavis directory, make sure the directory is called pretrained.

Morover, download bert-base-japanese-whole-word-masking weights and config from [the hugging face link](https://huggingface.co/cl-tohoku/bert-base-japanese-whole-word-masking/tree/main)

You should now be able to run the example.ipynb notebook. 

For directory naming conventions, you can also refer to the .gitignore file. 

## Use Case: Generate Japanese Captions for Captioning Datasets

Captions generated for [flickr30k dataset](https://www.kaggle.com/datasets/adityajn105/flickr30k?select=Images) can be found in flickr30k_caption.json. Script in flickr30k_caption_generate.ipynb. 

These captions are generated using top-k sampling instead of nucleus.

Captions generated by the pretrained and finetuned models are shown below:

![1001773457](https://github.com/ZhaoPeiduo/BLIP2-Japanese/assets/77187494/eae2e401-9697-45ad-b118-4c8ea7ae95f4)

 pretrained: {'image': '1001773457.jpg', 'caption': ['二 匹 の 犬 が 道路 で フリスビー を し て いる']} # No frisbee
 finetuned: {'image': '1001773457.jpg', 'caption': ['二 匹 の 犬 が 道路 で 喧嘩 を し て いる']}

 ![1001573224](https://github.com/ZhaoPeiduo/BLIP2-Japanese/assets/77187494/9a563146-e815-49e7-96d4-55a69a3d0123)
 
pretrained: {'image': '1001573224.jpg', 'caption': ['6 人 の 女性 が 屋内 で 飛び跳ね て いる']} # Wrong head count
finetuned: {'image': '1001573224.jpg', 'caption': ['黒い 服 を 着 た 女性 たち が 飛び跳ね て いる']}

In general, captions generated by the finetuned model is more accurate. 

## Use Case: Image Retrieval

Refer to the example.ipynb notebooks for more details. The idea is to get the average cosine similarity of query tokens between the image embeddings and the multimodal embeddings.

## Model training

The model was trained on a single GTX4080 GPU(laptop). Hence the config during training is modified as follows:

In blip2_pretrain.yaml: vit_precision = 'fp16'

In pretrain_stage1.yaml: batch_size = 25

During evaluation you have to change vit_precision back to fp32. 
