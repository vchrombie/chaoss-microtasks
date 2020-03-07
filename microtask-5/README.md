## microtask-5

Execute micro-mordred to collect, enrich and visualize data from Git and GitHub repositories.

The main step for this microtask is to setup the dev environment which is explained in [microtask-4](/microtask-4).

---

The configurations files (setup.cfg and project.json) for each backend varies.

### git

Commits from Git

projects.json
```
{
    "Chaoss": {
        "git": [
            "https:/github.com/chaoss/grimoirelab-perceval",
            "https:/github.com/chaoss/grimoirelab-sirmordred"
        ]
    }
}
```

setup.cfg
```
[git]
raw_index = git_raw
enriched_index = git_enriched
latest-items = true (suggested)
studies = [enrich_demography:git, enrich_git_branches:git, enrich_areas_of_code:git, enrich_onion:git, enrich_extra_data:git] (optional)

[enrich_demography:git] (optional)

[enrich_git_branches:git] (optional)

[enrich_areas_of_code:git] (optional)
in_index = git_raw
out_index = git-aoc_enriched

[enrich_onion:git] (optional)
in_index = git_enriched
out_index = git-onion_enriched

[enrich_extra_data:git]
json_url = https://gist.githubusercontent.com/zhquan/bb48654bed8a835ab2ba9a149230b11a/raw/5eef38de508e0a99fa9772db8aef114042e82e47/bitergia-example.txt

[enrich_forecast_activity]
out_index = git_study_forecast
```

### github

Issues and PRs from GitHub

**issue**

projects.json
```
{
    "Chaoss": {
        "github:issue": [
            "https:/github.com/chaoss/grimoirelab-perceval",
            "https:/github.com/chaoss/grimoirelab-sirmordred"
        ]
    }
}
```

setup.cfg
```
[github:issue]
raw_index = github_raw
enriched_index = github_enriched
api-token = xxxx
category = issue
sleep-for-rate = true
no-archive = true (suggested)
studies = [enrich_onion:github, 
           enrich_geolocation:user, 
           enrich_geolocation:assignee, 
           enrich_extra_data:github,
           enrich_backlog_analysis] (optional)

[enrich_onion:github] (optional)
in_index_iss = github_issues_onion-src
in_index_prs = github_prs_onion-src
out_index_iss = github-issues-onion_enriched
out_index_prs = github-prs-onion_enriched

[enrich_geolocation:user] (optional)
location_field = user_location
geolocation_field = user_geolocation

[enrich_geolocation:assignee] (optional)
location_field = assignee_location
geolocation_field = assignee_geolocation

[enrich_extra_data:github]
json_url = https://gist.githubusercontent.com/zhquan/bb48654bed8a835ab2ba9a149230b11a/raw/5eef38de508e0a99fa9772db8aef114042e82e47/bitergia-example.txt

[enrich_backlog_analysis]
out_index = github_enrich_backlog
interval_days = 7
reduced_labels = [bug,enhancement]
map_label = [others, bugs, enhancements]
```

**pull request**

projects.json
```
{
    "Chaoss": {
        "github:pull": [
            "https:/github.com/chaoss/grimoirelab-perceval",
            "https:/github.com/chaoss/grimoirelab-sirmordred"
        ]
    }
}
```

setup.cfg
```
[github:pull]
raw_index = github-pull_raw
enriched_index = github-pull_enriched
api-token = xxxx
category = pull_request
sleep-for-rate = true
no-archive = true (suggested)
studies = [enrich_geolocation:user, enrich_geolocation:assignee, enrich_extra_data:github] (optional)

[enrich_geolocation:user]
location_field = user_location
geolocation_field = user_geolocation

[enrich_geolocation:assignee]
location_field = assignee_location
geolocation_field = assignee_geolocation

[enrich_extra_data:github]
json_url = https://gist.githubusercontent.com/zhquan/bb48654bed8a835ab2ba9a149230b11a/raw/5eef38de508e0a99fa9772db8aef114042e82e47/bitergia-example.txt
```

**repo**: The number of forks, starts, and subscribers in the repository.

projects.json
```
{
    "Chaoss": {
        "github:repo": [
            "https:/github.com/chaoss/grimoirelab-perceval",
            "https:/github.com/chaoss/grimoirelab-sirmordred"
        ]
    }
}
```

setup.cfg
```
[github:repo]
raw_index = github-repo_raw
enriched_index = github-repo_enriched
api-token = xxxx
category = repository
sleep-for-rate = true
no-archive = true (suggested)
studies = [enrich_extra_data:github]

[enrich_extra_data:github]
json_url = https://gist.githubusercontent.com/zhquan/bb48654bed8a835ab2ba9a149230b11a/raw/5eef38de508e0a99fa9772db8aef114042e82e47/bitergia-example.txt
```
---

### Code:

```
micro.py --raw --enrich --cfg ./setup.cfg --backends <backend-name>
```
It executes the raw and enrich tasks for the cfg using the elasticsearch for the specific `backend-name` (git/github).

```
micro.py --panels --cfg ./setup.cfg
```
It executes the panels task to load the Sigils panels to Kibiter.

### Result

#### git



#### github


