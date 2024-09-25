# fast-cot
![](https://img.shields.io/badge/Python-3.9-brightgreen.svg)

No-strings tiny **[Chain-of-Thought](https://arxiv.org/abs/2201.11903) framework** for your Large Language Model (LLM) that saves you time ⏰ and money 💰

The end goal of this framework is to serve chain of prompts (a.k.a. [Chain-of-Thought](https://arxiv.org/abs/2201.11903)) 
formed into `schema` towards LLM.
It iterates through your data stored in `CSV`/`JSONL`/`sqlite`.

### Features
* ✅ **No-strings**: you're free to LLM dependencies and flexible `venv` customization.
* ✅ **Provides iterator over infinite amount of input contexts** served in `CSV`/`JSONL`.
* ✅ **Progress caching**: withstanding exception during LLM calls by using `sqlite3` engine for caching LLM answers;
* ✅ **Support schemas descriptions** for Chain-of-Thought concept.

# Installation

```bash
pip install git+https://github.com/nicolay-r/fast-cot@master
```

## Chain-of-Thought Schema

To declare Chain-of-Though (CoT) schema, this project exploits `JSON` format.
This format adopts `name` field for declaring a name and `schema` is a list of CoT instructions for the Large Language Model.

Each step represents a dictionary with `prompt` and `out` keys that corresponds to the input prompt and output variable name respectively.
All the variable names are expected to be mentioned in `{}`.

Below, is an example on how to declare your own schema:

```python
{
"name": "schema-name",
"schema": [
    {"prompt": "Given the question '{text}', let's think step-by-step.", 
     "out": "steps"},
    {"prompt": "For the question '{text}' the reasoining steps are '{steps}'. what would be an answer?", 
     "out":  "answer"},
]
}
```

Another templates are available [here](/ext/schema/thor_cot_schema.json).

# Usage

Just **three** simple steps:

1. Define your [CoT Schema](#chain-of-thought-schema), or fetch it as shown below:
```bash
!wget https://raw.githubusercontent.com/nicolay-r/fast-cot/refs/heads/master/ext/schema/default.json
```
2. Fetch or write your own **model** or pick the one [preset here](/ext/):
```bash
!wget https://raw.githubusercontent.com/nicolay-r/fast-cot/refs/heads/master/ext/flan_t5.py
```

3. Launch inference:
```bash
python infer.py --model "dynamic:flan_t5.py:FlanT5" \
      --schema "default.json" --device "cpu" --temp 0.1
```

# Embed your LLM

All you have to do is to implement `BaseLM` class, that includes:
* `__init__` -- for initialization;
* `ask(prompt)` -- infer your model with the given `prompt`.

See examples with models [here](/ext).
