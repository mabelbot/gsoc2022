# Coding Period Week 1 + 2 Report
[![Generic badge](https://img.shields.io/badge/Report_Status-In_Progress-<>.svg)](https://shields.io/)
[![Generic badge](https://img.shields.io/badge/Last_Updated_(PDT)-June_30,_2022-e10b95.svg)](https://shields.io/)

Quick link to this page if viewed elsewhere: https://github.com/mabelbot/gsoc2022/blob/main/work/week-03/week-03-progress.md

The organization I am using for this project: https://github.com/chaoss-conversion-rate-mabel



## Notes
- Set up the Pycharm sirmordred virtual environment properly
- Traced the enricher code with debugger
- This week I suffered from a medical emergency so I will be working over the weekend.
- Go through MM template https://github.com/grimoirelab-gitee/metrics_model
    - According to the README.md, there is a conf.yaml file which you'll use for initializing the metrics model.
    - This is going to be included in the `__init__` method of a class called `MetricModel` and also in the subclasses, in this case it's called `Activity_MetricsModel(MetricsModel)` in the example for Activity MM. These classes are defined in https://github.com/grimoirelab-gitee/metrics_model/blob/main/metric_model.py
    - You can include the json file that contains the links to the repos/projects/ etc. in the repository too, and pass its name into the conf.yaml file.
    - Method `def metrics_model_enrich(self,repos_list, label):` is on 339 which performs the enriching. 
    - It seems like you can bulk_upload data in dictionary form back to the ElasticSearch. 
    - 
    - 

# Bug Log
- `ModuleNotFoundError: No module named 'perceval.backend'` when running micro.py
    - Module perceval is correctly imported as a Content Root. However, it is not being recognized as a Python package (a python package, e.g. contains `__init__.py`)
    - I found `__init__.py` is not included in commits past June 1, 2022. 
    - For now I will put the old version of `__init__.py` back into the perceval directory under grimoirelab-perceval directory so we can continue to trace the code.
    - Then there is another error, ModuleNotFoundError: No module named 'perceval.backends.core'
    - For this, the backends also doesn't have an `__init__.py`.  
    - Now, the puppet error (ModuleNotFoundError: No module named 'perceval.backends.puppet') is back. I will comment out some lines in `/Users/mabel/Desktop/GSOC2022/grimoirelab-dev/sources/grimoirelab-elk/grimoire_elk/utils.py` to avoid it.
    - These are temporary "fixes" - it should be permanently fixed by changing imports or adding more `__init__.py` files.
    - `/Users/mabel/Desktop/GSOC2022/grimoirelab-dev/sources/grimoirelab-elk/grimoire_elk/utils.py` also claims on line 234 NameError: name 'Crates' is not defined.
        - Along with other connectors. I did comment those out for now. 
    - Repos changed - grimoirelab-elk, grimoirelab-perceval - change to temp branch and stash later when rebasing fixes. 
- sortinghat.exceptions.DatabaseError: Access denied for user 'root'@'localhost' (using password: NO) (err: 1045) when running micro.py
    - This did not happen before. I tried resetting the docker containers with `docker-compose down -v`. This is because I believe the docker-compose file was incomplete from the tutorial on the https://chaoss.github.io/grimoirelab-tutorial/docs/getting-started/dev-setup/ so once I fixed that, I was able to proceed. There also was a port 3306 conflict which I solved by removing the mysqld process that I have from a previous project. 


# Questions


    
# To Do List


# Random 
