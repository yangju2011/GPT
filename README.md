# GPT

## Playground for OpenAI
### Set up
1. Install python version >=3.7

`CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" pyenv install 3.11.2`

2. Create virtual env

`pyenv virtualenv 3.11.2 open-ai-3.11.2`

3. Activate virtual env

`pyenv activate open-ai-3.11.2`

4. Install packages from requirements.txt

`pip install -r requirements.txt`

## Fine tune with openai
1. run the notebook [openai_finetune_v2.ipynb](openai_finetune_v2.ipynb) and generate the data `openai_ad_script_demo_v2.jsonl`
2. terminal command to prepare the data
```agsl
openai tools fine_tunes.prepare_data -f <file_name>.jsonl -q
```
expect to see this in the log:
```agsl
Analyzing...
...
```

3. terminal command to fine tune 
```agsl
openai api fine_tunes.create -t "<file_name>.jsonl" -m <model> --suffix "<customized_model_name>"
```
- start with a cheaper model (ada) with fewer examples
- expect to see the cost and log. 
- it could take ~2 hours to wait for the queue.
- in this example, fine tuning takes about 30 seconds for 3 samples. 

```agsl
[2023-03-01 13:34:09] Created fine-tune: ft-ZYoZE35NQOuTkRICFVbsHuQl
[2023-03-01 13:39:54] Fine-tune costs $0.00
[2023-03-01 13:39:54] Fine-tune enqueued
[2023-03-01 14:15:14] Fine-tune is in the queue. Queue number: 31
[2023-03-01 14:16:58] Fine-tune is in the queue. Queue number: 30
...
[2023-03-01 15:13:42] Fine-tune is in the queue. Queue number: 0
[2023-03-01 15:18:39] Fine-tune started
[2023-03-01 15:18:53] Completed epoch 1/4
[2023-03-01 15:18:54] Completed epoch 2/4
[2023-03-01 15:18:54] Completed epoch 3/4
[2023-03-01 15:18:55] Completed epoch 4/4
[2023-03-01 15:19:11] Uploaded model: ada:ft-ad-ml:ad-script-demo-2023-03-01-20-19-10
[2023-03-01 15:19:11] Uploaded result file: file-oRVkWzOtICuw2Ptub9Ndd7s6
[2023-03-01 15:19:11] Fine-tune succeeded

Job complete! Status: succeeded 🎉
Try out your fine-tuned model:

openai api completions.create -m ada:ft-ad-ml:ad-script-demo-2023-03-01-20-19-10 -p <YOUR_PROMPT>
```

### Error message when using openai
1. `Fine-tune failed. Fine-tune will exceed billing hard limit`

This is caused by hitting the billing limit set by the org. 
Contact the org's person of contact to learn more about the billing limit. 
2. `Stream interrupted (client disconnected).`

This happens quite often when using fine tune api, follow the intruction to resume the stream

```
openai api fine_tunes.follow -i <job_id>
```

## Learning note from [Neural Networks: Zero to Hero](https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ)
1. [build backpropagation from scratch](build_backpropagation_from_scratch.ipynb)
2. [build a bigram language model from scratch](build_bigram_from_scratch.ipynb)
3. [build MLP for feature vector from scratch](built_MLP_for_feature_vector_from_scratch.ipynb)

...TBC