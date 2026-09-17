# plnEp1

## Pipelines com sklearn.pipeline.Pipeline e joblib
### Passo a passo para pipeline usando sklearn.pipeline.Pipeline

1. Testar o modelo que você quer colocar na pipeline

2. Construir a pipeline

Exemplo de preparação de pipeline puxado de baselineModel.ipynb, onde é criada uma pipeline para um Dummy Classifier:

```
pipelineDummy = Pipeline(
    steps=[
        ("clf", DummyClassifier(strategy='most_frequent', random_state = 100,
constant = None))
    ]
)
```

Se a pipeline exigir algum tipo específico de transformação dos dados (encoding etc), fazer um passo de preparação como:

```
preprocessor = ColumnTransformer(
    transformers=[
        ("num", StandardScaler(), numeric),
        ("cat", OneHotEncoder(drop="first"), categorical),
    ],
    remainder="drop"
)
```

E adicionar um passo 'prep' na pipeline:

```
pipelineDummy = Pipeline(
    steps=[
        ("prep", preprocessor),
        ("clf", DummyClassifier(strategy='most_frequent', random_state = 100,
constant = None))
    ]
)
```

3. Usando joblib, salvar a pipeline na pasta correta:

```
import joblib

joblib.dump(pipelineDummy, "pipelines/pipelineDummy.pkl")
```

### Colocando um modelo na com pipeline

1. Rode a pipeline sobre x_train e y_train para treinar o modelo

```
pipelineDummy.fit(x_train, y_train)
```

2. Rode 'predict' sobre x_test para usar o modelo

```
y_pred = pipelineDummy.predict(x_test)
```

3. Compare com y_test e pegue as métricas para avaliar o modelo

```
score = f1_score(y_test, y_pred, average='macro')
print("\n\nMaj =====>{:2.2f}\n\n".format(score).replace(".", ","))
```

### Pegando um modelo de uma pipeline
1. Pegue o modelo com joblib

```
import joblib

model = joblib.load("pipelines/pipelineDummy.pkl")
```

2. Use esse modelo para criar a previsão desejada

```
prediction = model.predict(x_test)
```
