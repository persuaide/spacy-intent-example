## spaCy demo: Intent Detection

This is a demo project. To run everything simply run;

```
python -m spacy project run train
python -m spacy project run evaluate
```

If you like the results, you can package the project. 

```
python -m spacy package ./training/model-best ./packages --name intentmod --version 0.1.0
cd ./packages/en_intentmod-0.1.0
pip install dist/en_intentmod-0.1.0.tar.gz
```

You can now load the model! 

```python
import spacy
nlp = spacy.load("en_intentmod")
```


<details>
  <summary><b>Pandas Conversion Script.</b></summary>
This is what we've internally used to turn the `.csv` file into `.jsonl`.
  
```python
import pandas as pd 

df = pd.read_csv("data/outofscope-intent-classification-dataset.csv")
X_train, X_test, y_train, y_test = train_test_split(df['text'], 
                                                    df['label'], 
                                                    test_size=5000, 
                                                    stratify=df['label'], 
                                                    random_state=42)

df_train = pd.DataFrame({'text': X_train, 'label': y_train})
df_test = pd.DataFrame({'text': X_test, 'label': y_test})
df_train.to_json("spacy-experiments/intent-benchmark/train.jsonl", orient="records")
df_test.to_json("spacy-experiments/intent-benchmark/test.jsonl", orient="records")
```

</details>
