# Coding Period Week 1 + 2 Report
[![Generic badge](https://img.shields.io/badge/Report_Status-In_Progress-<>.svg)](https://shields.io/)
[![Generic badge](https://img.shields.io/badge/Last_Updated_(PDT)-June_23,_2022-e10b95.svg)](https://shields.io/)

Quick link to this page if viewed elsewhere: https://github.com/mabelbot/gsoc2022/blob/main/work/week-01-02/week-01-02-progress.md

The organization I am using for this project: https://github.com/chaoss-conversion-rate-mabel


## Summary / TLDR
- Week 1 - consisted of meetings to scope out the project. See meeting notes from the two meetings that were held here: https://github.com/mabelbot/gsoc2022/blob/main/meetings/06-17-2022.md and https://github.com/mabelbot/gsoc2022/blob/main/meetings/06-19-2022.md
- Week 2 - more meetings to scope out the project. See meeting notes https://github.com/mabelbot/gsoc2022/blob/main/meetings/06-20-2022.md, https://github.com/mabelbot/gsoc2022/blob/main/meetings/06-21-2022.md
- Reviewed https://chaoss.github.io/grimoirelab-tutorial/docs/components/scenarios/ with Taiwei for data collection. Will review Taiwei proposal.
- I investigated the difference between our MM goal and existing "studies" in relation to conversion rate MM in grimoire lab. 
    - Answer: "Not yet, yes, conversion rate I hope is the first one. Yes, it is different from study, but you get reference from it . It is kinda like third level data analysis. First is raw data , second is enrich data , third is study. In my company we have implemented, but I hope take GSOC chance, we archive agreement upon Taiwei and your work."
- From speaking to Sean that although progress seems slow we have in fact learned a lot of real-world open source lessons already dealing with an unstructured problem and logistics issues during the bonding period. It is important that I get what I want out of the GSOC period and this is even at the cost of not finishing the project or making anything that "works" - although, we will strive for that.  
- Created my tracking repository https://github.com/mabelbot/gsoc2022/ with inspiration from @vchrombie 
- I created my organization as described to start contributing
    - Since this was my first time creating an organization I read a few resources to understand what to do, here's one of them: https://resources.github.com/downloads/github-guide-to-organizations.pdf


## Verbose Updates 
- Going back through the tutorial https://chaoss.github.io/grimoirelab-tutorial/docs/getting-started/dev-setup/. I also believe this is equivalent to https://github.com/chaoss/grimoirelab-sirmordred/blob/master/Getting-Started.md (see https://github.com/chaoss/community/issues/305) (to make sure I didn't miss anything during bonding period)
    - The option I had used previously was "Source code and Docker". I used the Elasticsearch without SearchGuard option.
    - Might need to redo this entire tutorial because of the organization (I did not use an organization before when I did the tutorial). Here are the steps I did
        1. Creating a `docker-compose.yml` file according to the tutorial.
        2. Run `docker-compose up -d` in directory `grimoirelab-dev` to get Elasticsearch, Kibiter and MariaDB running on your system. (Docker Desktop 4.9.1 (updated - took around 20 mins) in my case should be running).
            The output should be 
            ``` 
                Creating grimoirelab-dev_elasticsearch_1 ... done
                Creating grimoirelab-dev_mariadb_1       ... done
                Creating grimoirelab-dev_kibiter_1       ... done 
            ```

        3. [Cloning the repositories](https://chaoss.github.io/grimoirelab-tutorial/docs/getting-started/dev-setup/#cloning-the-repositories) - this step is going to fork and clone the repositories to a local folder. I created a virtual environment in `grimoirelab-dev` (dev-env-setup) (python3 -m venv dev-env-setup because I anticipate it will be different than the one in the Pycharm step later) to perform the installation `python3 -m pip install PyGitHub GitPython`. 
        4. I then followed the instructions to use the script. The script is in the Github Gist by @vchrombie, [glab-dev-env-setup](https://gist.github.com/vchrombie/4403193198cd79e7ee0079259311f6e8). I am going to try to modify it to take an organization parameter. This is because although I believe it can be done manually, [see here](https://stackoverflow.com/questions/9023533/fork-as-organization-after-already-forking-in-github#:~:text=Clicking%20the%20Fork%20button%20will,repository%20in%20your%20organization%20area.), but the script was really useful before.
            - https://pygithub.readthedocs.io/en/latest/github_objects/Repository.html
            - **Note**: I am not sure which of the repos within Grimoirelab I will ultimately need in the organization, nor how to exclude some in the organization and still keep the dev setup functioning the same way as the tutorial, so I will try to fork all of them.
            - I identified the `create_fork` method call that I want to replace (in class Organization - which is NOT the same `create_fork` as currently called in the script - that one is from class `AuthenticatedUser`, because there are multiple `create_fork` methods). 
                - From the Github API documentation for "Create a fork" we have
                    ```
                    Body parameters
                        Name, Type, Description
                        organizationstring
                        Optional parameter to specify the organization name if forking into an organization.

                    ``` 
                    This aligns with what we want. See https://pygithub.readthedocs.io/en/latest/examples/MainClass.html?highlight=organization#get-organization-by-name. 
                - It ended up being relatively straightforward to specify the organization by name. I then added ability pass the organization name in with the `-org` parameter. I was able to update the script's `create` method but I didn't touch the `update` method, it doesn't seem like it needs to be edited These edits are labeled with `a91e6c`.
                - I originally ran the script in the wrong directory, so I deleted sources folder it created and had to make a new organization. 
            - Final run of the script: `python3 glab-dev-env-setup.py --create --token [token] --source ../sources --org chaoss-conversion-rate-mabel`
            - The script I made is in this progress tracker under scripts. Use it at your own risk (it has not been 100% debugged yet)
        6. Check the remotes to make sure they are correct.
            - Testing out committing to one of the forked repos. I will try adding something to the readme of grimoirelab-elk to sanity check git push origin mabelbot-testing-06232022. Delete with `git branch -D mabelbot-testing-06232022 Deleted branch mabelbot-testing-06232022 (was fb0c9a76).`
            - I think the `origin` remote is not set correctly (it could still work that way, but it's connected to my own Github account repos). Therefore for grimoirelab-elk it should be `git remote set-url origin https://github.com/chaoss-conversion-rate-mabel/grimoirelab-elk`
            - Try again with the readme. Made a new branch `git checkout -b mabelbot-testing-06232022-origin` 
            - It seems to work now. So the script would need to add an extra step of setting the url of the origin remote as it is defaulting to the user's own repository IF you choose to work with an organization. These changes are marked with `f2c29d`. Referenced: https://gitpython.readthedocs.io/en/stable/reference.html?highlight=set_url#git.remote.Remote.set_url
        7. PyCharm setup. See the microtasks repository for what I did for this.
            - Create project from existing sources
            - The first environment got screwed up because I accidentally did not exclude here  "PyCharm will automatically create a virtual env, where you should install the dependencies listed in each requirements.txt, excluding the ones concerning the grimoirelab components." 
            - So I deleted it and made a new one and installed the requirements properly
            - I had an environment `grimoirelab-sirmordred2` from before, but this one is just `grimoirelab-sirmordred`




# Questions
1. When to run the script with `--update`? Is it still relevant?
2. Are remotes correct for an organization setup?
3. Tests for grimoirelab-elk
4. Is ignoring the correct behavior for the installation? This didn't come up last time
5. Issue ERROR: Could not find a version that satisfies the requirement grimoireelk (unavailable) (from versions: none) / ERROR: No matching distribution found for grimoireelk (unavailable)
6. 



    
# To Do List
- Finish round 2 of dev setup
- Finish review of https://github.com/chaoss/grimoirelab-elk/blob/master/grimoire_elk/enriched/github.py
- Review Yehui examples (TBD - not sent yet)
- Review proposal
- Customize readmes, organization
- Fix Payoneer issue for GSOC
- https://www.atlassian.com/git/tutorials/git-forks-and-upstreams
- Move blog post 0 from private to public



# Random 
- `/Users/mabel/.virtualenvs/grimoirelab-sirmordred/bin/python -m pip install --upgrade pip `