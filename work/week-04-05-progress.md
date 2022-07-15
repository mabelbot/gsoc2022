# Coding Period Week 4 and 5 Report
[![Generic badge](https://img.shields.io/badge/Report_Status-In_Progress-<>.svg)](https://shields.io/)
[![Generic badge](https://img.shields.io/badge/Last_Updated_(PDT)-July_8,_2022-e10b95.svg)](https://shields.io/)


The organization I am using for this project: https://github.com/chaoss-conversion-rate-mabel



# Notes

### Completing Custom Enricher Code for Github Issues (as separate file - `githubcr.py`)
- Completing enrichment (additional fields, modifying fields)
- From this enricher, we get data from 
    - Info about Issue creation by the issue `author`. We do not need information about issue closing as the githubql enricher has info on it.
    - Info about assignees but not when they are assigned
    - Reactions on the issue

### Completing Custom Enricher Code for Github Issues (on top of - `githubql.py`)
- Exploring data needed: timeline data with `githubql`
- Thanks to pointer from Yehui, I have figured out that the timeline (events) is actually available from the githubql enricher. Only these events are available (compared to the full list of event types on the Github API)
```  
    'ADDED_TO_PROJECT_EVENT',
    'MOVED_COLUMNS_IN_PROJECT_EVENT',
    'REMOVED_FROM_PROJECT_EVENT',
    'CROSS_REFERENCED_EVENT',
    'LABELED_EVENT',
    'UNLABELED_EVENT',
    'CLOSED_EVENT'
```
- There is not just who performed the above actions (the "actor"), but we also have
    - A `reporter`
    - An `author`
    - 'merge'
    - 'reference'
    - 'board'
    - A `closer`
    - A `submitter`
    - For now, we will not be using this info but later TODO can include them.
- Conclusion: I do not need to write my own enricher for timeline data. I may need to add some more. 
- In elasticsearch, the indices will not be in any specific order (so timeline for a single issue will not return in consecutive order with the Python client)
    - Uniquely identify an issue with: `_source.issue_url`
    - User identification: `_source.actor_username`
    - In some cases, an event involves more than 1 person, such as "assigned"/"unassigned": account for the assignee [note that this info is NOT given by githubql.py]
    - 
-  

### Finding information on reactions on Github Issues

### grimoirelab-metricsmodels
- This is another submodule for the Conversion Rate and other Metrics Models that will be added to GL
- I spent some time figuring out the right way to package it, the requirements necessary, and how to use Poetry with it.
- I have made my conf.yaml file (slightly different from the one used by the gitee Community Activity MM)
- I have also made a conversion_rate.py file based on metric_model.py (inheriting from the main class) to contain the conversion rate.
    - I am omitting some parameters for now and adding others


# Bug Log
- bulk_upload resulting in 0 uploads
    - This is the same when appending to an new index or clean bulk uploading into a brand new index. I'm also messing up the serialization
    - It's not working with the exact same fields.
    - new_items in elastic.py is 0, so this could be the source of the bug.
    - Stepping through the debugger, I see the error is in `safe_put_bulk(self, url, bulk_json):`
    - The errors are of type
    ```
    {"index":{"_index":"test_index","_type":"items","_id":"55866f23f9236267a6e4b71379fc5cf485be2fc0","status":400,"error":{"type":"mapper_parsing_exception","reason":"Field [_index] is a metadata field and cannot be added inside a document. Use the index API request parameters."}}},
    ```
    - To fix this, I will remove all metadata and turn _id into uuid. I will also remove _source from all fields (this was an artifact from json_normalize in pandas, which should not be explicitly included an Elasticsearch document).
    - Bug resolved - we are now able to upload via bulk_upload.
- 

# Branches Tracker
- See google doc



# Questions
Do I need to care about the other connectors?
Where does control flow after the enricher?
What is repo option for?
Layered calculations
How to write tests
Where are the log statements coming from? e.g. Collection for github: starting..." (grimoirelab-sirmordred/sirmordred/task_collection.py) 
what as *github for
can you explain metadata annotation (i need to learn more about this myself anyway)
what happens if you change the rich schema after an incremental load?
rate limit
how to work the logger
how to use issue_comment_reactions in perceval
is the first connector a perceval connector




    
# To Do List
- Check Yehui's development environment tutorial
- Check setup.cfg, projects.json, https://github.com/chaoss/grimoirelab-sirmordred/
- Move code to metrics models repository
- Poetry, https://dev.to/codemouse92/dead-simple-python-project-structure-and-imports-38c6, https://www.geeksforgeeks.org/python-call-parent-class-method/
- How to write a good readme for the metrics models repository
- meeting notes transcribe
- Add logging
- Need to upgrade pandas to >1.0.0 instead of the current pandas requirements in the virutal environment 


# Random 
- 