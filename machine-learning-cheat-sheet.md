# Machine Learning
:TODO: python framework mataflow `from metaflow import FlowSpec, step `
:TODO: python framework sklearn `from sklearn.pipeline import Pipeline`

```mermaid
graph LR;
    A[<b>Data In</b>
    * filtered
    * cleaned
    * labeled
    ] --> 
    B(<b>ML Algorithm</b>
    * frameworks
    * algorithms
    🔄️
    );

    B --> 
    C(<b>Data out</b>
    ✅️ example 🆗️
    )    
```
---
```mermaid
graph 

m[<b>model</b>]
t[training]
i[inference]
t --associate--> m
i --associate--> m

r[regression
  model]
c[classification
  model]
c --extend--> m
r --extend--> m

l[label]
l --assign 
    to -->m

id[input data]
f[feature]
f --o id

idl[ <b>input data</b>
     labeled
     for training]
idnl[<b>input data</b>
    not labeled
    for prediction]
idl --extend--> id

idnl --extend--> id 
l --o idl

id ~~~ i
```

## Necessary knowledges
```mermaid
graph LR;
    d[design] --> md[model <br>development] --> o[operations]
    md --> d 
    o --> md
```
### design
  * Requirement engineering
  * ML UseCases prioritization
  * Data Availability Check
### model development
  * Data Engineering
  * ML Model Engineering
  * Model Testing & Validation
### operations
  * ML Model Deployment
  * CI/CD pipelines
  * Monitoring & triggering

## Frameworks
* ['double' ML](https://github.com/py-why/EconML/blob/main/README.md)
  `pip install econml`
* [Google Jupyter Notebook](https://colab.research.google.com/)
