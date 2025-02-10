# nano-r1

Taste the "aha moment" for $12 in 3 hours
- Uses [Unsloth](https://unsloth.ai) to run reinforcement learning on [Qwen 2.5 3B](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct) to create a mini reasoning model
- The model also forgets all its censorship 🌚

![chart](./images/chart.png)

![chart](./images/prompt1.png)

## Environment

- Hardware: 1X H200 ([RunPod](https://www.runpod.io))
- Python: 3.11.10
- PyTorch: 2.5.1+cu124
- CUDA: 12.4

## Info

This is a modified version of Unsloth's [Qwen2.5_(3B)-GRPO notebook](https://colab.research.google.com/github/unslothai/notebooks/blob/main/nb/Qwen2.5_(3B)-GRPO.ipynb#scrollTo=DkIvEkIIkEyB).

- Changed `MAX_COMPLETION_LENGTH` from 200 to 1024; otherwise the model will learn to miminize answer length because the `<answer>` won't be within 200 tokens and reward will always be 0
- Increase `MAX_SEQ_LENGTH` to account for longer completion length
- Changed system prompt to encourage deeper thinking
- Modified reward functions
  - Update `correctness_reward`
    - search for answer within the completion rather than requiring the response to be exactly the answer, so reward is still given for responses like `$100` or `100 ounces`
    - allow tiny differences (e.g. 4.0 vs 4.00000001)
  - Remove `int_reward` since the new `correctness_reward` makes this not needed
  - Update `strict_format_reward` and `soft_format_reward`
    - Added `re.DOTALL` to allow newlines within responses
    - Don't require trailing newline in strict format
  - Added `length_reward` to encourage more thinking
    - applies to tokens within `<reasoning>` tags only
- Use older typing_extensions to prevent version mismatch on some non-Colab environments

## How to run

- Create a [RunPod](https://www.runpod.io) account (or other GPU cloud provider)
- Create a [wandb](https://wandb.ai) account (for logging)
- Launch 1X H200 instance and open a Jupyter session
- Copy the cells in `nano-r1.ipynb` into your cloud notebook
- Run the first `!pip install` cell
- Refresh the notebook
- Run the cells up to `wandb.init()`
- Get your wandb API key, paste it in after running `wandb.init()`
- Run the other cells
- Wait 3 hours
- Profit!

## Example prompts

### Reasoning

![chart](./images/prompt2.png)

![chart](./images/prompt3.png)

![chart](./images/prompt4.png)

### Uncensored

![chart](./images/prompt5.png)

![chart](./images/prompt6.png)

![chart](./images/prompt7.png)

### humans are bad

![chart](./images/prompt8.png)

### lol

![chart](./images/prompt9.png)

