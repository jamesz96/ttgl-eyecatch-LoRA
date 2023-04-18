# ttgl-eyecatch-LoRA


This [LoRA](https://github.com/cloneofsimo/lora) model aims to reproduce the unique art style seen in the [eyecatches](https://en.wiktionary.org/wiki/eyecatch) from the 2007 anime "Tengen Toppa Gurren Lagann".





<details>
    <summary>Eyecatch Examples</summary>

![3 - 2QkYa2B](https://user-images.githubusercontent.com/30682722/232262899-65313e59-a40e-442e-ae8c-41cb38ac40e8.jpg)

![6 - q4JT7xT](https://user-images.githubusercontent.com/30682722/232262910-06ff5a3a-c5d0-4f3b-8943-168f2897b454.jpg)

![10 - AaXN8Nd](https://user-images.githubusercontent.com/30682722/232264086-7570efaf-4872-4d37-997e-d2ebbe6bfb47.jpg)

![30 - V4aXp3E](https://user-images.githubusercontent.com/30682722/232262932-64e91711-83ed-4396-a803-66a2ba87bc9c.jpg)


Eyecatch Illustration Credits:
- Akira Amemiya (eps 5, 7, 22)
- Atsushi Nishigori (4 episodes)
- Chikashi Kubota (ep 14)
- Hiroyuki Imaishi (eps 1, 18)
- Ikuo Kuwana (ep 24)
- Katsuzo Hirata (ep 10)
- Kazuhiro Takamura (ep 12)
- Kikuko Sadakata (eps 5, 22)
- Kouichi Motomura (4 episodes)
- Mitsuru Ishihara (ep 3)
- Osamu Kobayashi (ep 4)
- Satoshi Yamaguchi (eps 20, 25)
- Shingo Abe (eps 13, 21, 26)
- Shōko Nakamura (ep 10)
- Sunao Chikaoka (ep 3)
- Sushio (ep 15)
- Tadashi Hiramatsu (ep 26)
- Takashi Mukouda (ep 9)
- Yamato Kojima (ep 23)
- Yoh Yoshinari (ep 27)
- Yuka Shibata (eps 6, 21)
</details>


### Usage Details

- Easiest way to get set up is to use [AUTO1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
  - There are plenty of guides on this online
- [Download me](https://civitai.com/models/44734?modelVersionId=49372) from Civit.ai
- Best results on [AnyLoRA](https://civitai.com/models/23900/anylora-checkpoint) checkpoint


<br />

| AnyLora Checkpoint | ttgl-eyecatch |
| --- | --- |
| ![00083-4214451159](https://user-images.githubusercontent.com/30682722/232762400-d5d3921c-89e1-4585-83d2-4c5a14cebc8a.png) | ![00087-4214451159](https://user-images.githubusercontent.com/30682722/232768708-f3f71a0e-27cf-4e21-96ac-fbfcc3154847.png)
| ![00032-3030562801-no-lora](https://user-images.githubusercontent.com/30682722/232760120-a056b256-9a03-4554-92ca-eb11d3284281.png) | ![00031-3030562801](https://user-images.githubusercontent.com/30682722/232760164-dcf90844-45a6-4801-860c-a33b35afa077.png) |
| ![00040-3030562801](https://user-images.githubusercontent.com/30682722/232760459-5f83c2fb-9eb7-4600-9ea6-3ceec73483ff.png) | ![00054-3030562801](https://user-images.githubusercontent.com/30682722/232760498-bc365440-fc85-4991-af0f-3794da42658c.png) |
![00204-2477855727](https://user-images.githubusercontent.com/30682722/232782078-7b447098-3539-4926-815b-026053e4d74f.png) | ![00203-2477855727](https://user-images.githubusercontent.com/30682722/232782111-41229c10-22ba-4319-80eb-3f73690f8dd8.png) |

____

# More details for those interested

### Training Data

I extracted 28 `512x512` images from ~50% of the eyecatches used in the show. The remaining eyecatches used a different style or were tricky to caption so they weren't included in the training set.

Cleaning the data involved:
- removing watermarks
- manual captioning (training for a general art style not a specific style or object)
  - Describe EVERYTHING in the image EXCEPT for the style
  - Still on the fence as to whether I should include "soft" triggers
  - Omit names from all captions
- inpainting
- cropping (a few cases of extracting multiple images from a single eyecatch)

### Training

Stable Diffusion checkpoint: [anyloraCheckpoint_bakedvaeFtmseFp16NO](https://civitai.com/models/23900/anylora-checkpoint).

Recommended LoRA optimizer args gave me the best results, though I probably haven't played around with these parameters enough yet.

<details>
    <summary>Training config used for [Kohya-ss](https://github.com/kohya-ss/sd-scripts)</summary>

```
[additional_network_arguments]
no_metadata = false
unet_lr = 0.0001
text_encoder_lr = 5e-5
network_module = "networks.lora"
network_dim = 32
network_alpha = 16
network_train_unet_only = false
network_train_text_encoder_only = false

[optimizer_arguments]
optimizer_type = "AdamW8bit"
learning_rate = 0.0001
max_grad_norm = 1.0
lr_scheduler = "constant"
lr_warmup_steps = 0

[dataset_arguments]
debug_dataset = false
in_json = ***
train_data_dir = ***
dataset_repeats = 15
shuffle_caption = true
keep_tokens = 0
resolution = "512,512"
caption_dropout_rate = 0
caption_tag_dropout_rate = 0
caption_dropout_every_n_epochs = 0
color_aug = false
token_warmup_min = 1
token_warmup_step = 0

[training_arguments]
output_dir = ***
output_name = "ttgl-eyecatch-original"
save_precision = "fp16"
save_every_n_epochs = 1
train_batch_size = 6
max_token_length = 225
mem_eff_attn = false
xformers = true
max_train_epochs = 10
max_data_loader_n_workers = 8
persistent_data_loader_workers = true
gradient_checkpointing = false
gradient_accumulation_steps = 1
mixed_precision = "fp16"
clip_skip = 2
logging_dir = ***
log_prefix = "ttgl-eyecatch-original"
lowram = true
```
</details>


### Epoch / STR Plot

![](https://user-images.githubusercontent.com/30682722/232767950-3fb07c17-a9d7-4752-8fc4-2a464222a3bf.png)

In my opinion, the eyecatch art style I'm looking for is most prominent in models (columns) `000005` and `000007` from strengths `0.5` to `1`.  

I'll go with the weaker one for now.


Choosing one over the other is probably not a big deal as something like [LoRA Block Weight](https://github.com/hako-mikan/sd-webui-lora-block-weight) extension for [AUTO1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) allows setting the strength at each individual block as opposed to applying the same strength across all blocks.


### What's Next?

This is model is trained purely on the original eyecatches. There are only 54 of them in total which isn't a whole lot.
Fortunately, there are hundreds if not thousands of these [Gurren Lagann eyecatch parodies](https://knowyourmeme.com/memes/gurren-lagann-eyecatch-parodies) created by fans.

> *Fun sidenote: these "parodies" are really just memes but this was a time before memes were called '"memes" - so the name stuck.*

I've done a little experimenting with including these community eyecatches in the training set but the resulting models definitely need some more tweaking.