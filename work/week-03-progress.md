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
- Reinstalling Perceval due to issue with release 0.19.1
    - Perceval changes stashed - Saved working directory and index state WIP on master: 75a0633 Release 0.19.1
- Line 240 -  https://github.com/chaoss/grimoirelab-elk/blob/master/grimoire_elk/utils.py#L240 "github": [GitHub, GitHubOcean, GitHubEnrich, GitHubCommand],
- Github and GitHubCommand are coming from import from `perceval.backends.core.github import GitHub, GitHubCommand`. GitHubOcean is from `from .raw.github import GitHubOcean` (raw). GitHubEnrich is from `from .enriched.github import GitHubEnrich`. 
- class githubEnrich is the starting point of the github enricher https://github.com/chaoss/grimoirelab-elk/blob/cf433d69356cffffa79b475876147687f1bfb1cd/grimoire_elk/enriched/github.py#L99
- From now I believe the structure of this project should be multiple enrichers > MM (skipping the study level).
- I'm working through github.py to see how an enricher works. 
- The current config file and run configurations are for `git` and not `github`, but I will investigate that first.
- What is in the raw data (`git-chaoss`)?
    - It only contains commits.
- Next, I tried changing the config file to do [github] enrichment with data coming from Perceval repository
- What is in the raw data (`github_issues_chaoss`)?
    - Issue, PR, or repository? Seems like only "issues"
    - I believe some columns are duplicates? (Actually not)
    - There are a lot of columns


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
- Building with glab env setup py - it actually goes through the code (old versions) that I have forked locally (i.e. to mabelbot) on a create (update rebases normally). I have to delete those and modify the line where there's a bug in glab-dev-env-setup.py, line 179. 
- Reinstalling modules does not fix this bug. 
- Install GitToolBox plugin for autofetch on Pycharm

# Branches Tracker
- New branch: `github_mm_enricher` for grimoirelab-elk
- Files changed in sirmordred: projects.json, setup.cfg
- Files changed in grimoirelab-elk = grimoirelab-elk/grimoire_elk/utils.py
- Files changed in perceval - add init.pys 



# Questions
- Go over bugs. Current status of rebased project / old version
- In projects.json - *github vs github
- What's in github:pull github:repo
- Is git chaoss only supposed to contain commits
- What happens if you specify something else in category = issue? Like if you tried `pull_request` instead?
- How does the mapping fit in?
- Where do the studies fit in? What is the point of calling them? Are they necessary?
- What is a connector?
- What's in mariadb? Do we need any code handling there? Maybe you need to update the identities to make sure they are using the same convention
- Resolving double identities in GrimoireLab



    
# To Do List
- Check how the raw data works, and check how enriching will happen
- Make meeting notes readable
- Trace micro.py
- Create config file - explore using jupyter notebook
- Trace enricher code
- Create branches
- Add 1 thing to organization
- Create issue
- Use kibana for data insights!
- branch



# Random 
- Maintainer roles?
- sig = special interesting group = project
- Query methods elastic serach