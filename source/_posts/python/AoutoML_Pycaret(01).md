---
title: "Auto Machine Learning by pycaret(01)"
date: 2021-12-10 16:00:48
categories:
- python
- machineLeaning
- pycaret
tags:
- Study
- python
- pycaret
- machineLeaning
- auto Machine Learning
---

## AutoMachineLearning by pycaret
 
[pycaret](https://pycaret.org/)

![gitHub_pycaret](/../../imeges/python/gitHub_pycaret.png)

<br>
<hr>

### pycaret으로 autoML 하기 

- low-code machine learning library
- PyCaret 2.0  ver.
  - 분석가가 가야 하는 최종 도착지
  - 머신러닝 + operation (운영) : 배포 -> 
    + MLflow, Airflow, Kubeflow...
    

[gitHub and pycaret](https://towardsdatascience.com/github-is-the-best-automl-you-will-ever-need-5331f671f105)

<br><br>

---

### pycaret install 

```python
!pip install pycaret


# !pip install pycaret==2.0
```

> Collecting pycaret
  Downloading pycaret-2.3.5-py3-none-any.whl (288 kB)
     |████████████████████████████████| 288 kB 5.4 MB/s 
Collecting lightgbm>=2.3.1
  Downloading lightgbm-3.3.1-py3-none-manylinux1_x86_64.whl (2.0 MB)
     |████████████████████████████████| 2.0 MB 54.5 MB/s 
Collecting pyod
  Downloading pyod-0.9.5.tar.gz (113 kB)
     |████████████████████████████████| 113 kB 67.4 MB/s 
Requirement already satisfied: textblob in /usr/local/lib/python3.7/dist-packages (from pycaret) (0.15.3)
Requirement already satisfied: yellowbrick>=1.0.1 in /usr/local/lib/python3.7/dist-packages (from pycaret) (1.3.post1)
Collecting Boruta
  Downloading Boruta-0.3-py3-none-any.whl (56 kB)
     |████████████████████████████████| 56 kB 4.6 MB/s 
Collecting pyLDAvis
  Downloading pyLDAvis-3.3.1.tar.gz (1.7 MB)
     |████████████████████████████████| 1.7 MB 63.6 MB/s 
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
    Preparing wheel metadata ... done
Requirement already satisfied: seaborn in /usr/local/lib/python3.7/dist-packages (from pycaret) (0.11.2)
Requirement already satisfied: nltk in /usr/local/lib/python3.7/dist-packages (from pycaret) (3.2.5)
Collecting imbalanced-learn==0.7.0
  Downloading imbalanced_learn-0.7.0-py3-none-any.whl (167 kB)
     |████████████████████████████████| 167 kB 65.5 MB/s 
Requirement already satisfied: numpy==1.19.5 in /usr/local/lib/python3.7/dist-packages (from pycaret) (1.19.5)
Collecting kmodes>=0.10.1
  Downloading kmodes-0.11.1-py2.py3-none-any.whl (19 kB)
Requirement already satisfied: spacy<2.4.0 in /usr/local/lib/python3.7/dist-packages (from pycaret) (2.2.4)
Collecting umap-learn
  Downloading umap-learn-0.5.2.tar.gz (86 kB)
     |████████████████████████████████| 86 kB 4.8 MB/s 
Requirement already satisfied: IPython in /usr/local/lib/python3.7/dist-packages (from pycaret) (5.5.0)
Requirement already satisfied: matplotlib in /usr/local/lib/python3.7/dist-packages (from pycaret) (3.2.2)
Requirement already satisfied: wordcloud in /usr/local/lib/python3.7/dist-packages (from pycaret) (1.5.0)
Requirement already satisfied: cufflinks>=0.17.0 in /usr/local/lib/python3.7/dist-packages (from pycaret) (0.17.3)
Collecting mlflow
  Downloading mlflow-1.22.0-py3-none-any.whl (15.5 MB)
     |████████████████████████████████| 15.5 MB 68.5 MB/s 
Collecting mlxtend>=0.17.0
  Downloading mlxtend-0.19.0-py2.py3-none-any.whl (1.3 MB)
     |████████████████████████████████| 1.3 MB 66.9 MB/s 
Requirement already satisfied: pandas in /usr/local/lib/python3.7/dist-packages (from pycaret) (1.1.5)
Collecting scikit-plot
  Downloading scikit_plot-0.3.7-py3-none-any.whl (33 kB)
Requirement already satisfied: joblib in /usr/local/lib/python3.7/dist-packages (from pycaret) (1.1.0)
Requirement already satisfied: plotly>=4.4.1 in /usr/local/lib/python3.7/dist-packages (from pycaret) (4.4.1)
Collecting scikit-learn==0.23.2
  Downloading scikit_learn-0.23.2-cp37-cp37m-manylinux1_x86_64.whl (6.8 MB)
     |████████████████████████████████| 6.8 MB 37.1 MB/s 
Requirement already satisfied: gensim<4.0.0 in /usr/local/lib/python3.7/dist-packages (from pycaret) (3.6.0)
Collecting pandas-profiling>=2.8.0
  Downloading pandas_profiling-3.1.0-py2.py3-none-any.whl (261 kB)
     |████████████████████████████████| 261 kB 60.5 MB/s 
Requirement already satisfied: ipywidgets in /usr/local/lib/python3.7/dist-packages (from pycaret) (7.6.5)
Requirement already satisfied: scipy<=1.5.4 in /usr/local/lib/python3.7/dist-packages (from pycaret) (1.4.1)
Requirement already satisfied: threadpoolctl>=2.0.0 in /usr/local/lib/python3.7/dist-packages (from scikit-learn==0.23.2->pycaret) (3.0.0)
Requirement already satisfied: setuptools>=34.4.1 in /usr/local/lib/python3.7/dist-packages (from cufflinks>=0.17.0->pycaret) (57.4.0)
Requirement already satisfied: colorlover>=0.2.1 in /usr/local/lib/python3.7/dist-packages (from cufflinks>=0.17.0->pycaret) (0.3.0)
Requirement already satisfied: six>=1.9.0 in /usr/local/lib/python3.7/dist-packages (from cufflinks>=0.17.0->pycaret) (1.15.0)
Requirement already satisfied: smart-open>=1.2.1 in /usr/local/lib/python3.7/dist-packages (from gensim<4.0.0->pycaret) (5.2.1)
Requirement already satisfied: pickleshare in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (0.7.5)
Requirement already satisfied: pexpect in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (4.8.0)
Requirement already satisfied: traitlets>=4.2 in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (5.1.1)
Requirement already satisfied: pygments in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (2.6.1)
Requirement already satisfied: simplegeneric>0.8 in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (0.8.1)
Requirement already satisfied: prompt-toolkit<2.0.0,>=1.0.4 in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (1.0.18)
Requirement already satisfied: decorator in /usr/local/lib/python3.7/dist-packages (from IPython->pycaret) (4.4.2)
Requirement already satisfied: nbformat>=4.2.0 in /usr/local/lib/python3.7/dist-packages (from ipywidgets->pycaret) (5.1.3)
Requirement already satisfied: widgetsnbextension~=3.5.0 in /usr/local/lib/python3.7/dist-packages (from ipywidgets->pycaret) (3.5.2)
Requirement already satisfied: ipython-genutils~=0.2.0 in /usr/local/lib/python3.7/dist-packages (from ipywidgets->pycaret) (0.2.0)
Requirement already satisfied: ipykernel>=4.5.1 in /usr/local/lib/python3.7/dist-packages (from ipywidgets->pycaret) (4.10.1)
Requirement already satisfied: jupyterlab-widgets>=1.0.0 in /usr/local/lib/python3.7/dist-packages (from ipywidgets->pycaret) (1.0.2)
Requirement already satisfied: jupyter-client in /usr/local/lib/python3.7/dist-packages (from ipykernel>=4.5.1->ipywidgets->pycaret) (5.3.5)
Requirement already satisfied: tornado>=4.0 in /usr/local/lib/python3.7/dist-packages (from ipykernel>=4.5.1->ipywidgets->pycaret) (5.1.1)
Requirement already satisfied: wheel in /usr/local/lib/python3.7/dist-packages (from lightgbm>=2.3.1->pycaret) (0.37.0)
Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib->pycaret) (3.0.6)
Requirement already satisfied: python-dateutil>=2.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib->pycaret) (2.8.2)
Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib->pycaret) (1.3.2)
Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.7/dist-packages (from matplotlib->pycaret) (0.11.0)
Requirement already satisfied: jsonschema!=2.5.0,>=2.4 in /usr/local/lib/python3.7/dist-packages (from nbformat>=4.2.0->ipywidgets->pycaret) (2.6.0)
Requirement already satisfied: jupyter-core in /usr/local/lib/python3.7/dist-packages (from nbformat>=4.2.0->ipywidgets->pycaret) (4.9.1)
Requirement already satisfied: pytz>=2017.2 in /usr/local/lib/python3.7/dist-packages (from pandas->pycaret) (2018.9)
Collecting visions[type_image_path]==0.7.4
  Downloading visions-0.7.4-py3-none-any.whl (102 kB)
     |████████████████████████████████| 102 kB 12.4 MB/s 
Collecting pydantic>=1.8.1
  Downloading pydantic-1.8.2-cp37-cp37m-manylinux2014_x86_64.whl (10.1 MB)
     |████████████████████████████████| 10.1 MB 24.8 MB/s 
Collecting tangled-up-in-unicode==0.1.0
  Downloading tangled_up_in_unicode-0.1.0-py3-none-any.whl (3.1 MB)
     |████████████████████████████████| 3.1 MB 22.1 MB/s 
Collecting joblib
  Downloading joblib-1.0.1-py3-none-any.whl (303 kB)
     |████████████████████████████████| 303 kB 60.4 MB/s 
Collecting requests>=2.24.0
  Downloading requests-2.26.0-py2.py3-none-any.whl (62 kB)
     |████████████████████████████████| 62 kB 805 kB/s 
Requirement already satisfied: tqdm>=4.48.2 in /usr/local/lib/python3.7/dist-packages (from pandas-profiling>=2.8.0->pycaret) (4.62.3)
Collecting PyYAML>=5.0.0
  Downloading PyYAML-6.0-cp37-cp37m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (596 kB)
     |████████████████████████████████| 596 kB 42.5 MB/s 
Requirement already satisfied: missingno>=0.4.2 in /usr/local/lib/python3.7/dist-packages (from pandas-profiling>=2.8.0->pycaret) (0.5.0)
Requirement already satisfied: markupsafe~=2.0.1 in /usr/local/lib/python3.7/dist-packages (from pandas-profiling>=2.8.0->pycaret) (2.0.1)
Collecting htmlmin>=0.1.12
  Downloading htmlmin-0.1.12.tar.gz (19 kB)
Collecting multimethod>=1.4
  Downloading multimethod-1.6-py3-none-any.whl (9.4 kB)
Collecting phik>=0.11.1
  Downloading phik-0.12.0-cp37-cp37m-manylinux2010_x86_64.whl (675 kB)
     |████████████████████████████████| 675 kB 41.5 MB/s 
Requirement already satisfied: jinja2>=2.11.1 in /usr/local/lib/python3.7/dist-packages (from pandas-profiling>=2.8.0->pycaret) (2.11.3)
Requirement already satisfied: networkx>=2.4 in /usr/local/lib/python3.7/dist-packages (from visions[type_image_path]==0.7.4->pandas-profiling>=2.8.0->pycaret) (2.6.3)
Requirement already satisfied: attrs>=19.3.0 in /usr/local/lib/python3.7/dist-packages (from visions[type_image_path]==0.7.4->pandas-profiling>=2.8.0->pycaret) (21.2.0)
Requirement already satisfied: Pillow in /usr/local/lib/python3.7/dist-packages (from visions[type_image_path]==0.7.4->pandas-profiling>=2.8.0->pycaret) (7.1.2)
Collecting imagehash
  Downloading ImageHash-4.2.1.tar.gz (812 kB)
     |████████████████████████████████| 812 kB 37.7 MB/s 
Collecting scipy<=1.5.4
  Downloading scipy-1.5.4-cp37-cp37m-manylinux1_x86_64.whl (25.9 MB)
     |████████████████████████████████| 25.9 MB 74.1 MB/s 
Requirement already satisfied: retrying>=1.3.3 in /usr/local/lib/python3.7/dist-packages (from plotly>=4.4.1->pycaret) (1.3.3)
Requirement already satisfied: wcwidth in /usr/local/lib/python3.7/dist-packages (from prompt-toolkit<2.0.0,>=1.0.4->IPython->pycaret) (0.2.5)
Requirement already satisfied: typing-extensions>=3.7.4.3 in /usr/local/lib/python3.7/dist-packages (from pydantic>=1.8.1->pandas-profiling>=2.8.0->pycaret) (3.10.0.2)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /usr/local/lib/python3.7/dist-packages (from requests>=2.24.0->pandas-profiling>=2.8.0->pycaret) (1.24.3)
Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.7/dist-packages (from requests>=2.24.0->pandas-profiling>=2.8.0->pycaret) (2021.10.8)
Requirement already satisfied: charset-normalizer~=2.0.0 in /usr/local/lib/python3.7/dist-packages (from requests>=2.24.0->pandas-profiling>=2.8.0->pycaret) (2.0.8)
Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.7/dist-packages (from requests>=2.24.0->pandas-profiling>=2.8.0->pycaret) (2.10)
Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (3.0.6)
Requirement already satisfied: srsly<1.1.0,>=1.0.2 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (1.0.5)
Requirement already satisfied: wasabi<1.1.0,>=0.4.0 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (0.8.2)
Requirement already satisfied: thinc==7.4.0 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (7.4.0)
Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (1.0.6)
Requirement already satisfied: plac<1.2.0,>=0.9.6 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (1.1.3)
Requirement already satisfied: catalogue<1.1.0,>=0.0.7 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (1.0.0)
Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (2.0.6)
Requirement already satisfied: blis<0.5.0,>=0.4.0 in /usr/local/lib/python3.7/dist-packages (from spacy<2.4.0->pycaret) (0.4.1)
Requirement already satisfied: importlib-metadata>=0.20 in /usr/local/lib/python3.7/dist-packages (from catalogue<1.1.0,>=0.0.7->spacy<2.4.0->pycaret) (4.8.2)
Requirement already satisfied: zipp>=0.5 in /usr/local/lib/python3.7/dist-packages (from importlib-metadata>=0.20->catalogue<1.1.0,>=0.0.7->spacy<2.4.0->pycaret) (3.6.0)
Requirement already satisfied: notebook>=4.4.1 in /usr/local/lib/python3.7/dist-packages (from widgetsnbextension~=3.5.0->ipywidgets->pycaret) (5.3.1)
Requirement already satisfied: terminado>=0.8.1 in /usr/local/lib/python3.7/dist-packages (from notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (0.12.1)
Requirement already satisfied: nbconvert in /usr/local/lib/python3.7/dist-packages (from notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (5.6.1)
Requirement already satisfied: Send2Trash in /usr/local/lib/python3.7/dist-packages (from notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (1.8.0)
Requirement already satisfied: pyzmq>=13 in /usr/local/lib/python3.7/dist-packages (from jupyter-client->ipykernel>=4.5.1->ipywidgets->pycaret) (22.3.0)
Requirement already satisfied: ptyprocess in /usr/local/lib/python3.7/dist-packages (from terminado>=0.8.1->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (0.7.0)
Requirement already satisfied: PyWavelets in /usr/local/lib/python3.7/dist-packages (from imagehash->visions[type_image_path]==0.7.4->pandas-profiling>=2.8.0->pycaret) (1.2.0)
Collecting querystring-parser
  Downloading querystring_parser-1.2.4-py2.py3-none-any.whl (7.9 kB)
Requirement already satisfied: entrypoints in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (0.3)
Collecting alembic<=1.4.1
  Downloading alembic-1.4.1.tar.gz (1.1 MB)
     |████████████████████████████████| 1.1 MB 66.5 MB/s 
Collecting gitpython>=2.1.0
  Downloading GitPython-3.1.24-py3-none-any.whl (180 kB)
     |████████████████████████████████| 180 kB 40.6 MB/s 
Requirement already satisfied: protobuf>=3.7.0 in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (3.17.3)
Requirement already satisfied: sqlalchemy in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (1.4.27)
Requirement already satisfied: Flask in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (1.1.4)
Requirement already satisfied: cloudpickle in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (1.3.0)
Collecting databricks-cli>=0.8.7
  Downloading databricks-cli-0.16.2.tar.gz (58 kB)
     |████████████████████████████████| 58 kB 5.6 MB/s 
Collecting prometheus-flask-exporter
  Downloading prometheus_flask_exporter-0.18.6-py3-none-any.whl (17 kB)
Requirement already satisfied: packaging in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (21.3)
Requirement already satisfied: click>=7.0 in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (7.1.2)
Requirement already satisfied: sqlparse>=0.3.1 in /usr/local/lib/python3.7/dist-packages (from mlflow->pycaret) (0.4.2)
Collecting gunicorn
  Downloading gunicorn-20.1.0-py3-none-any.whl (79 kB)
     |████████████████████████████████| 79 kB 7.6 MB/s 
Collecting docker>=4.0.0
  Downloading docker-5.0.3-py2.py3-none-any.whl (146 kB)
     |████████████████████████████████| 146 kB 58.9 MB/s 
Collecting Mako
  Downloading Mako-1.1.6-py2.py3-none-any.whl (75 kB)
     |████████████████████████████████| 75 kB 4.2 MB/s 
Collecting python-editor>=0.3
  Downloading python_editor-1.0.4-py3-none-any.whl (4.9 kB)
Requirement already satisfied: tabulate>=0.7.7 in /usr/local/lib/python3.7/dist-packages (from databricks-cli>=0.8.7->mlflow->pycaret) (0.8.9)
Collecting websocket-client>=0.32.0
  Downloading websocket_client-1.2.3-py3-none-any.whl (53 kB)
     |████████████████████████████████| 53 kB 1.2 MB/s 
Collecting gitdb<5,>=4.0.1
  Downloading gitdb-4.0.9-py3-none-any.whl (63 kB)
     |████████████████████████████████| 63 kB 1.6 MB/s 
Collecting smmap<6,>=3.0.1
  Downloading smmap-5.0.0-py3-none-any.whl (24 kB)
Requirement already satisfied: greenlet!=0.4.17 in /usr/local/lib/python3.7/dist-packages (from sqlalchemy->mlflow->pycaret) (1.1.2)
Requirement already satisfied: itsdangerous<2.0,>=0.24 in /usr/local/lib/python3.7/dist-packages (from Flask->mlflow->pycaret) (1.1.0)
Requirement already satisfied: Werkzeug<2.0,>=0.15 in /usr/local/lib/python3.7/dist-packages (from Flask->mlflow->pycaret) (1.0.1)
Requirement already satisfied: bleach in /usr/local/lib/python3.7/dist-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (4.1.0)
Requirement already satisfied: pandocfilters>=1.4.1 in /usr/local/lib/python3.7/dist-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (1.5.0)
Requirement already satisfied: testpath in /usr/local/lib/python3.7/dist-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (0.5.0)
Requirement already satisfied: mistune<2,>=0.8.1 in /usr/local/lib/python3.7/dist-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (0.8.4)
Requirement already satisfied: defusedxml in /usr/local/lib/python3.7/dist-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (0.7.1)
Requirement already satisfied: webencodings in /usr/local/lib/python3.7/dist-packages (from bleach->nbconvert->notebook>=4.4.1->widgetsnbextension~=3.5.0->ipywidgets->pycaret) (0.5.1)
Requirement already satisfied: prometheus-client in /usr/local/lib/python3.7/dist-packages (from prometheus-flask-exporter->mlflow->pycaret) (0.12.0)
Collecting pyLDAvis
  Downloading pyLDAvis-3.3.0.tar.gz (1.7 MB)
     |████████████████████████████████| 1.7 MB 44.1 MB/s 
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
    Preparing wheel metadata ... done
  Downloading pyLDAvis-3.2.2.tar.gz (1.7 MB)
     |████████████████████████████████| 1.7 MB 30.5 MB/s 
Requirement already satisfied: numexpr in /usr/local/lib/python3.7/dist-packages (from pyLDAvis->pycaret) (2.7.3)
Requirement already satisfied: future in /usr/local/lib/python3.7/dist-packages (from pyLDAvis->pycaret) (0.16.0)
Collecting funcy
  Downloading funcy-1.16-py2.py3-none-any.whl (32 kB)
Requirement already satisfied: numba>=0.35 in /usr/local/lib/python3.7/dist-packages (from pyod->pycaret) (0.51.2)
Requirement already satisfied: statsmodels in /usr/local/lib/python3.7/dist-packages (from pyod->pycaret) (0.10.2)
Requirement already satisfied: llvmlite<0.35,>=0.34.0.dev0 in /usr/local/lib/python3.7/dist-packages (from numba>=0.35->pyod->pycaret) (0.34.0)
Requirement already satisfied: patsy>=0.4.0 in /usr/local/lib/python3.7/dist-packages (from statsmodels->pyod->pycaret) (0.5.2)
Collecting pynndescent>=0.5
  Downloading pynndescent-0.5.5.tar.gz (1.1 MB)
     |████████████████████████████████| 1.1 MB 55.1 MB/s 
Building wheels for collected packages: htmlmin, imagehash, alembic, databricks-cli, pyLDAvis, pyod, umap-learn, pynndescent
  Building wheel for htmlmin (setup.py) ... done
  Created wheel for htmlmin: filename=htmlmin-0.1.12-py3-none-any.whl size=27098 sha256=6dff1694390dae41ea8bd3ca00f5564142023ea037fa606be0a8ffba9c16d1da
  Stored in directory: /root/.cache/pip/wheels/70/e1/52/5b14d250ba868768823940c3229e9950d201a26d0bd3ee8655
  Building wheel for imagehash (setup.py) ... done
  Created wheel for imagehash: filename=ImageHash-4.2.1-py2.py3-none-any.whl size=295207 sha256=9e38b104e77871b6f6a6a9267c3debd3ac85d39441acb3cda64d4dc07a11dd27
  Stored in directory: /root/.cache/pip/wheels/4c/d5/59/5e3e297533ddb09407769762985d134135064c6831e29a914e
  Building wheel for alembic (setup.py) ... done
  Created wheel for alembic: filename=alembic-1.4.1-py2.py3-none-any.whl size=158172 sha256=652d8b88b2468cf1d1c9f1c3242dda689e0f670bc6e5b88dc4dbf087fecbaccc
  Stored in directory: /root/.cache/pip/wheels/be/5d/0a/9e13f53f4f5dfb67cd8d245bb7cdffe12f135846f491a283e3
  Building wheel for databricks-cli (setup.py) ... done
  Created wheel for databricks-cli: filename=databricks_cli-0.16.2-py3-none-any.whl size=106811 sha256=9dbaaca3ece5f6a1522d676d8b1ec35c065a0bd0c564bd862ae3012984b70c9a
  Stored in directory: /root/.cache/pip/wheels/f4/5c/ed/e1ce20a53095f63b27b4964abbad03e59cf3472822addf7d29
  Building wheel for pyLDAvis (setup.py) ... done
  Created wheel for pyLDAvis: filename=pyLDAvis-3.2.2-py2.py3-none-any.whl size=135618 sha256=471d50a9a2e725465ffc7d32b21edb15c9b84dc0573891185a43e97f567aa0a7
  Stored in directory: /root/.cache/pip/wheels/f8/b1/9b/560ac1931796b7303f7b517b949d2d31a4fbc512aad3b9f284
  Building wheel for pyod (setup.py) ... done
  Created wheel for pyod: filename=pyod-0.9.5-py3-none-any.whl size=132699 sha256=d116f5b46155bf0fa31aa88cc21da0e3be461b448e9c9b2d599c763a5ef0a6a1
  Stored in directory: /root/.cache/pip/wheels/3d/bb/b7/62b60fb451b33b0df1ab8006697fba7a6a49709a629055cf77
  Building wheel for umap-learn (setup.py) ... done
  Created wheel for umap-learn: filename=umap_learn-0.5.2-py3-none-any.whl size=82709 sha256=7c48e34d2c19d333a623ed12491d3c7d07bafd52f2d35e474df56908f5cc7525
  Stored in directory: /root/.cache/pip/wheels/84/1b/c6/aaf68a748122632967cef4dffef68224eb16798b6793257d82
  Building wheel for pynndescent (setup.py) ... done
  Created wheel for pynndescent: filename=pynndescent-0.5.5-py3-none-any.whl size=52603 sha256=7abff97eebc36deea7220f1b5e9907020826a07404003a9c7d794fef4d396e87
  Stored in directory: /root/.cache/pip/wheels/af/e9/33/04db1436df0757c42fda8ea6796d7a8586e23c85fac355f476
Successfully built htmlmin imagehash alembic databricks-cli pyLDAvis pyod umap-learn pynndescent
Installing collected packages: tangled-up-in-unicode, smmap, scipy, multimethod, joblib, websocket-client, visions, scikit-learn, requests, python-editor, Mako, imagehash, gitdb, querystring-parser, PyYAML, pynndescent, pydantic, prometheus-flask-exporter, phik, htmlmin, gunicorn, gitpython, funcy, docker, databricks-cli, alembic, umap-learn, scikit-plot, pyod, pyLDAvis, pandas-profiling, mlxtend, mlflow, lightgbm, kmodes, imbalanced-learn, Boruta, pycaret
  Attempting uninstall: scipy
    Found existing installation: scipy 1.4.1
    Uninstalling scipy-1.4.1:
      Successfully uninstalled scipy-1.4.1
  Attempting uninstall: joblib
    Found existing installation: joblib 1.1.0
    Uninstalling joblib-1.1.0:
      Successfully uninstalled joblib-1.1.0
  Attempting uninstall: scikit-learn
    Found existing installation: scikit-learn 1.0.1
    Uninstalling scikit-learn-1.0.1:
      Successfully uninstalled scikit-learn-1.0.1
  Attempting uninstall: requests
    Found existing installation: requests 2.23.0
    Uninstalling requests-2.23.0:
      Successfully uninstalled requests-2.23.0
  Attempting uninstall: PyYAML
    Found existing installation: PyYAML 3.13
    Uninstalling PyYAML-3.13:
      Successfully uninstalled PyYAML-3.13
  Attempting uninstall: pandas-profiling
    Found existing installation: pandas-profiling 1.4.1
    Uninstalling pandas-profiling-1.4.1:
      Successfully uninstalled pandas-profiling-1.4.1
  Attempting uninstall: mlxtend
    Found existing installation: mlxtend 0.14.0
    Uninstalling mlxtend-0.14.0:
      Successfully uninstalled mlxtend-0.14.0
  Attempting uninstall: lightgbm
    Found existing installation: lightgbm 2.2.3
    Uninstalling lightgbm-2.2.3:
      Successfully uninstalled lightgbm-2.2.3
  Attempting uninstall: imbalanced-learn
    Found existing installation: imbalanced-learn 0.8.1
    Uninstalling imbalanced-learn-0.8.1:
      Successfully uninstalled imbalanced-learn-0.8.1
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
google-colab 1.0.0 requires requests~=2.23.0, but you have requests 2.26.0 which is incompatible.
datascience 0.10.6 requires folium==0.2.1, but you have folium 0.8.3 which is incompatible.
albumentations 0.1.12 requires imgaug<0.2.7,>=0.2.5, but you have imgaug 0.2.9 which is incompatible.
Successfully installed Boruta-0.3 Mako-1.1.6 PyYAML-6.0 alembic-1.4.1 databricks-cli-0.16.2 docker-5.0.3 funcy-1.16 gitdb-4.0.9 gitpython-3.1.24 gunicorn-20.1.0 htmlmin-0.1.12 imagehash-4.2.1 imbalanced-learn-0.7.0 joblib-1.0.1 kmodes-0.11.1 lightgbm-3.3.1 mlflow-1.22.0 mlxtend-0.19.0 multimethod-1.6 pandas-profiling-3.1.0 phik-0.12.0 prometheus-flask-exporter-0.18.6 pyLDAvis-3.2.2 pycaret-2.3.5 pydantic-1.8.2 pynndescent-0.5.5 pyod-0.9.5 python-editor-1.0.4 querystring-parser-1.2.4 requests-2.26.0 scikit-learn-0.23.2 scikit-plot-0.3.7 scipy-1.5.4 smmap-5.0.0 tangled-up-in-unicode-0.1.0 umap-learn-0.5.2 visions-0.7.4 websocket-client-1.2.3


pycaret을 그냥 설치 할 수도 있고,
version을 정해서 설치 할 수도 있다. 

- 먼저 개괄적으로 확인만 하고, github 에서 autoML을 pycaret 2.0 ver로 진행 해야 한다. 

<br><br>

---

### pycaret install 