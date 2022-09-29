# CodeGen: An Open Large Language Model for Code with Multi-Turn Program Synthesis

This is a repository that hosts source code for the submission: [CodeGen: An Open Large Language Model for Code with Multi-Turn Program Synthesis](https://openreview.net/forum?id=iaYcJKpY2B_).

## 💡 Important Links 💡

* **Data**: [Multi-Turn Programming Benchmark](https://github.com/anonymous-researcher-1/codegen/blob/main/mtpb.jsonl)

* **Online Demo**: [demo.codegen-iclr.org](http://demo.codegen-iclr.org)

* **Model Outputs**: [benchmark.codegen-iclr.org](http://benchmark.codegen-iclr.org)

## Setup for Running Locally

```sh
# Clone the repository and cd to the directory.
# Assuming cloning is done:
cd CodeGen

# Download the model parameters
# codegen-350M-nl,multi,mono
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-350M-nl.tar.gz && tar -xvf checkpoints/codegen-350M-nl.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-350M-multi.tar.gz && tar -xvf checkpoints/codegen-350M-multi.tar.gz -C checkpoints/
wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-350M-mono.tar.gz && tar -xvf checkpoints/codegen-350M-mono.tar.gz -C checkpoints/
# codegen-2B-nl,multi,mono
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-2B-nl.tar.gz && tar -xvf checkpoints/codegen-2B-nl.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-2B-multi.tar.gz && tar -xvf checkpoints/codegen-2B-multi.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-2B-mono.tar.gz && tar -xvf checkpoints/codegen-2B-mono.tar.gz -C checkpoints/
# codegen-6B-nl,multi,mono
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-6B-nl.tar.gz && tar -xvf checkpoints/codegen-6B-nl.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-6B-multi.tar.gz && tar -xvf checkpoints/codegen-6B-multi.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-6B-mono.tar.gz && tar -xvf checkpoints/codegen-6B-mono.tar.gz -C checkpoints/
# codegen-16B-nl,multi,mono
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-16B-nl.tar.gz && tar -xvf checkpoints/codegen-16B-nl.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-16B-multi.tar.gz && tar -xvf checkpoints/codegen-16B-multi.tar.gz -C checkpoints/
# wget -P checkpoints https://storage.googleapis.com/codegen-anonymized/checkpoints/codegen-16B-mono.tar.gz && tar -xvf checkpoints/codegen-16B-mono.tar.gz -C checkpoints/

# create a virtual environment with requirements
python3.8 -m venv .venv
source .venv/bin/activate
pip3 install --upgrade pip setuptools
pip3 install -r requirements.txt

# sample from the model with an arbitrary context
python3 -m jaxformer.hf.sample --model codegen-350M-mono --context "def hello_world():"
```

## Released Models
We release models of various sizes trained on various datasets. The models are named in the following format:
```
codegen-{model-size}-{data}
```

`model-size` has 4 options: `350M`, `2B`, `6B`, `16B`, which represent the number of parameters in each model.

`data` has 3 options: `nl`, `multi`, `mono`.

* `nl` models are randomly initialized and trained on [The Pile](https://github.com/EleutherAI/the-pile), a 825.18 GB English text corpus.
* `multi` models are initialized from `nl` models and then trained on a corpus with code data consisting of multiple programming languages.
* `mono` models are initialized from `multi` models and then trained on a corpus with Python code data.

The model names can be provided to the `--model` flag for `sample.py`. See a sample usage above in Setup.

## Multi-Turn Programming Benchmark

We include our proposed benchmark with this repository as [`mtpb.jsonl`](https://github.com/anonymous-researcher-1/codegen/blob/main/mtpb.jsonl).
Each line is a problem expressed in JSON format, consisting of the following fields:

* `id`: Problem ID
* `name`: Problem name (cf. Appendix D)
* `description`: A short description of the problem
* `category`: Manually labeled problem category
* `prompts`: A list of template-enabled strings, specifying each step.
* `inputs`: A list consisting of 5 test case inputs. Each test case is a key-value table mapping the variables (used in the templated prompt) to actual values.
* `outputs`: A list consisting of 5 test case outputs. Each test case is an expected output value of the program.
* `max_gen_length`: Maximum number of tokens we set for each turn for the problem. The value is  mostly 128 because each turn doesn't require substantial lines of code, but we adjusted a higher number when long generation is expected.

## Model Outputs

We make all the model outputs for the proposed MTPB [here (anonymized link)](http://benchmark.codegen-iclr.org).
