## Introduction
CLIP, which stands for **C**ontrastive **L**anguage-**I**mage **P**re-training, is a deep learning model developed by OpenAI in 2021. 

==CLIP’s embeddings for images and text share the same space, enabling direct comparisons between the two modalities.== This is accomplished by training the model to:
 1. Bring related images and texts closer together, while 
 2. Pushing unrelated ones apart

### Potential Applications
1. Image Classification and Retrieval
	1. By associating images with natural language descriptions, it allows users to search for images using textual queries.
2. Content Moderation
	1. Analyze images and accompanying text to identify and filter out inappropriate or harmful content.

## Training Overview
CLIP is designed to predict ==which N × N potential (image, text) pairings within the batch are actual matches==. 

To achieve this, CLIP establishes a multi-modal embedding space through the **joint training of an image encoder and text encoder**.

> **The CLIP loss aims to maximize the cosine similarity between the image and text embeddings for the N genuine pairs in the batch while minimizing the cosine similarity for the N² − N incorrect pairings**

The optimization process involves using a **symmetric cross-entropy loss function** that operates on these similarity scores. 
## Model architecture

ClIP uses two separate architectures as the backbone for encoding vision and text datasets:

- `image_encoder`: Represents the neural network architecture (e.g., ResNet or Vision Transformer) responsible for encoding images.
- `text_encoder`: Represents the neural network architecture (e.g., CBOW, BERT, or Text Transformer) responsible for encoding textual information.


![[Pasted image 20241122220322.png | Architecture of the CLIP model]]

## Training Process
```Python
# image_encoder - ResNet or Vision Transformer  
# text_encoder - CBOW or Text Transformer  
# I[n, h, w, c] - minibatch of aligned images  
# T[n, l] - minibatch of aligned texts  
# W_i[d_i, d_e] - learned proj of image to embed  
# W_t[d_t, d_e] - learned proj of text to embed  
# t - learned temperature parameter  
# extract feature representations of each modality  
I_f = image_encoder(I) #[n, d_i]  
T_f = text_encoder(T) #[n, d_t]  

# joint multimodal embedding [n, d_e]  
I_e = l2_normalize(np.dot(I_f, W_i), axis=1)  
T_e = l2_normalize(np.dot(T_f, W_t), axis=1)

# scaled pairwise cosine similarities [n, n]  
logits = np.dot(I_e, T_e.T) * np.exp(t)  

# symmetric loss function  
labels = np.arange(n)  
loss_i = cross_entropy_loss(logits, labels, axis=0)  
loss_t = cross_entropy_loss(logits, labels, axis=1)  
loss = (loss_i + loss_t)/2
```


### Input Data
The model takes a batch of n pairs of images and texts as input where:

- `I[n, h, w, c]`: Represents a minibatch of aligned images, where `n` is the batch size, `h` is the image height, `w` is the image width, and `c` is the number of channels.
- `T[n, l]`: Represents a minibatch of aligned texts, where `n` is the batch size, and `l` is the length of the textual sequence.

### Feature Extraction
- `I_f = image_encoder(I)`: Extracts feature representations (`I_f`) from the image encoder. The shape of `I_f` is `[n, d_i]`, where `d_i` is the dimensionality of the image features.
- `T_f = text_encoder(T)`: Extracts feature representations (`T_f`) from the text encoder. The shape of `T_f` is `[n, d_t]`, where `d_t` is the dimensionality of the text features.

```Python
I_f = models.resnet34(pretrained=True) # for encoding images  
T_f= AutoModel.from_pretrained("distilbert-base-multilingual-cased") # for encoding captions
```
### Learned Projections
- `W_i[d_i, d_e]`: Represents the learned projection matrix for mapping image features (`I_f`) to an embedding space (`I_e`). The shape of `W_i` is `[d_i, d_e]`, where `d_e` is the desired dimensionality of the joint embedding space.
- `W_t[d_t, d_e]`: Represents the learned projection matrix for mapping text features (`T_f`) to the same embedding space (`T_e`). The shape of `W_t` is `[d_t, d_e]`

The projection operation can be coded using a neural network with two linear layers, whose weights are the **learned projection matrix.** 

> Additionally, the projection layer plays a ==crucial role in aligning the dimensions of image and text embeddings==, ensuring that they have the same size.

```Python
class Projection(nn.Module):
    def __init__(self, d_in: int, d_out: int, p: float=0.5) -> None:
        super().__init__()
        self.linear1 = nn.Linear(d_in, d_out, bias=False)
        self.linear2 = nn.Linear(d_out, d_out, bias=False)
        self.layer_norm = nn.LayerNorm(d_out)
        self.drop = nn.Dropout(p)

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        embed1 = self.linear1(x)
        embed2 = self.drop(self.linear2(F.gelu(embed1)))
        embeds = self.layer_norm(embed1 + embed2)
        return embeds
```
###  Embedding and Normalization

- `I_e = l2_normalize(np.dot(I_f, W_i), axis=1)`: Embeds and normalizes image features in the joint embedding space (`I_e`).
- `T_e = l2_normalize(np.dot(T_f, W_t), axis=1)`: Embeds and normalizes text features in the joint embedding space (`T_e`)

```Python
class VisionEncoder(nn.Module):
    def __init__(self, d_out: int) -> None:
        super().__init__()
        base = models.resnet34(pretrained=True)
        d_in = base.fc.in_features
        base.fc = nn.Identity()
        self.base = base
        self.projection = Projection(d_in, d_out)
        for p in self.base.parameters():
            p.requires_grad = False

    def forward(self, x):
        projected_vec = self.projection(self.base(x))
        projection_len = torch.norm(projected_vec, dim=-1, keepdim=True)
        return projected_vec / projection_len


class TextEncoder(nn.Module):
    def __init__(self, d_out: int) -> None:
        super().__init__()
        self.base = AutoModel.from_pretrained(Config.text_model)
        self.projection = Projection(Config.transformer_embed_dim, d_out)
        for p in self.base.parameters():
            p.requires_grad = False

    def forward(self, x):
        out = self.base(x)[0]
        out = out[:, 0, :]  # get CLS token output
        projected_vec = self.projection(out)
        projection_len = torch.norm(projected_vec, dim=-1, keepdim=True)
        return projected_vec / projection_len

vision_encoder = VisionEncoder(Config.embed_dim)  
I_e = vision_encoder(images)  
caption_encoder = TextEncoder(Config.embed_dim)  
T_e = caption_encoder(text["input_ids"])
```

### Cosine Similarities:
- `logits = np.dot(I_e, T_e.T) * np.exp(t)`: Computes pairwise cosine similarities between image and text embeddings, scaled by a learned temperature parameter `t`
### Symmetric Loss Function:

CLIP uses contrastive loss (first introduced in [Representation Learning with Contrastive Predictive Coding](https://arxiv.org/pdf/1807.03748.pdf)) to bring related images and texts closer together while pushing unrelated ones apart/

- `labels = np.arange(n)`: Generates labels representing the indices of the batch.
- `loss_i = cross_entropy_loss(logits, labels, axis=0)`: Computes the cross-entropy loss along the image axis.
- `loss_t = cross_entropy_loss(logits, labels, axis=1)`: Computes the cross-entropy loss along the text axis.
- `loss = (loss_i + loss_t)/2`: Computes the symmetric average of the image and text losses.

```Python
def CLIP_loss(logits: torch.Tensor) -> torch.Tensor:
    n = logits.shape[1]      # number of samples
    labels = torch.arange(n) # Create labels tensor
    # Calculate cross entropy losses along axis 0 and 1
    loss_i = F.cross_entropy(logits.transpose(0, 1), labels, reduction="mean")
    loss_t = F.cross_entropy(logits, labels, reduction="mean")
    # Calculate the final loss
    loss = (loss_i + loss_t) / 2

    return loss
```

### Final Model

```Python
class CustomModel(nn.Module):
    def __init__(self, lr: float = 1e-3) -> None:
        super().__init__()
        self.vision_encoder = VisionEncoder(Config.embed_dim)
        self.caption_encoder = TextEncoder(Config.embed_dim)
        self.tokenizer = Tokenizer(AutoTokenizer.from_pretrained(Config.text_model))
        self.lr = lr
        self.device = "cuda" if torch.cuda.is_available() else "cpu"

    def forward(self, images, text):
        text = self.tokenizer(text).to(self.device)

        image_embed = self.vision_encoder(images)
        caption_embed = self.caption_encoder(text["input_ids"])
        similarity = caption_embed @ image_embed.T

        loss = CLIP_loss(similarity)
        img_acc, cap_acc = metrics(similarity)
        return loss, img_acc, cap_acc
```

## Discussion

In a nutshell, this model learns the relationship between **a whole sentence** and the **image** it describes; in a sense that when the model is trained, given an input sentence it will be able to retrieve the most related images corresponding to that sentence.

The important thing here is that it is trained on full sentences instead of single classes like car, dog, etc. The intuition is that when trained on whole sentences, the model can learn a lot more things and finds some pattern between images and texts.

> [!faq] What are the trainable parameters in the CLIP model ? 
>  Image Encoder parameters, Text Encoder parameters, Projection Layer weights and biases.
>  


## References
1. https://towardsdatascience.com/clip-model-and-the-importance-of-multimodal-embeddings-1c8f6b13bf72
2. 















