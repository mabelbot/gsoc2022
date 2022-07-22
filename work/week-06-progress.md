# Coding Period Week 6 Report
[![Generic badge](https://img.shields.io/badge/Report_Status-In_Progress-<>.svg)](https://shields.io/)
[![Generic badge](https://img.shields.io/badge/Last_Updated_(PDT)-July_21,_2022-e10b95.svg)](https://shields.io/)


The organization I am using for this project: https://github.com/chaoss-conversion-rate-mabel


# Notes
- SH for inactive users could help identify inactive users for the unconversion rate.



# Bug Log: 
- Sorting hat for github issues is using the name+email while sorting hat for the github events is using the github login/username and it seems it has no access to an email address. So the only way is to match up using the usernames across these 2 github backends to get a full picture.
    - For example, altsalt and Wm Salt Hale (salt@altsalt.net) x2 are all 3 different profiles
        - And they have different uuids...
        - Matching the similar ones only brings up the two with the same email so that could be disambiguated given the data we have so far
    - We could merge them using the API, but not with the information we have so far. 
        - but I think more information should be put in in the first place to disambiguate these profiles 
        - The problem becomes where in the code do these users get put into sorting hat?
            - There are 2 methods inside github.py that could be useful (which mention "identity"):
                - def get_sh_identity(self, item, identity_field=None):
                - def get_identities(self, item): calls the above
                - In turn, get_identities is called in 2 places `grimoire_elk/elk.py` and `utils/gelk.py`
            - The code seems to be collecting the correct fields from `item` so this is suspicious.
            - Perhaps there is an issue with `class Profile(ModelBase):` not disambiguating name/username for GH

# Questions
- Is sorting hat incremental or over the entire data?
- Sorting hat UUID is the same over multiple reruns because it's a hash right?
- Lack of emails for github?
- What part of sorting hat "comes with" the execution of github and githubql enrich?
- Any way to make debug faster?
- What do we do about rate limit exhausted for collection github?
- Logging?
- How to run SH backend from micro
