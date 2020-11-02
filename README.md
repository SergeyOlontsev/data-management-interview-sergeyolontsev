Process for using individual projects as "take home" assignments

* Install GitHub's official CLI
* Save this script somewhere on your path and name it `takehome`:

```
#!/usr/bin/env bash

if [ -z "$1" ] ; then
   echo "Need to specify a name for the candidate"
   exit 1
fi

if [ -z "$2" ] ; then
    echo "Need to specify the candidate's github username"
    exit 2
fi

export GITHUB_USER=MY_GITHUB_USER_NAME
export GITHUB_TOKEN=MY_GITHUB_TOKEN
GUSER="$1"
CANDIDATE="$2"

BASE=${HOME}/pulsepoint/src/datamanagement
REPOBASE=data-management-interview
REPONAME=${REPOBASE}-${GUSER}
LOCALREPO=${BASE}/${REPOBASE}-candidates
cd ${BASE}/data-management-interview
cd ${LOCALREPO}
hub create --private --copy pulsepointinc/${REPONAME}
git remote -v 2>&1 | grep "^${GUSER}" >/dev/null 2>&1 && git remote remove ${GUSER}
git remote add ${GUSER} git@github.com:pulsepointinc/${REPONAME}.git
git push --set-upstream ${GUSER} master

hub api -t -X PUT /orgs/pulsepointinc/teams/data-mgmt/repos/pulsepointinc/${REPONAME} -F permission=admin
hub api -t -X PUT /orgs/pulsepointinc/teams/techops/repos/pulsepointinc/${REPONAME} -F permission=pull
hub api -t -X PUT /orgs/pulsepointinc/teams/exchange/repos/pulsepointinc/${REPONAME} -F permission=pull
hub api -t -X PUT /orgs/pulsepointinc/teams/data-and-analytics/repos/pulsepointinc/${REPONAME} -F permission=pull
hub api -t -X PUT /repos/pulsepointinc/${REPONAME}/collaborators/${CANDIDATE} -F permission=pull
```

* Change the `${BASE}` variable to point to the parent of the location where you will store the repository for candidate takehome assessments.
* Change the `${REPOBASE}` to the name of the directory under `${BASE}` where the takehome exists.

* You can now run `takehome ${CANDIDATE_NAME} ${CANDIDATE_GITHUB_NAME}` and this will create a new repository, push the takehome project into that repository, give everybody at Pulsepoint the needed permissions on that repository, and finally invite the candidate to that repository. It will also put the URL of the repository onto the clipboard for you.

* Send an e-mail to the candidate explaining the assignment:
    ```bash
    You should get a github invite to the
    https://github.com/pulsepointinc/data-management-interview-candidate
    repository. There's a README there that explains what this project
    does and how to run it. There are some failing tests; the goal is to
    get the tests to pass. There are no rules or limits. We do ask that
    you fork the project then work in a branch to fix tests. When
    completed, submit a Pull Request against the master branch of the
    original repository.
    ```
* Await a pull request, and pull it down locally, replacing <ID>
   below with the PR id number (typically 1 if there is only one PR
   against the candidate's repository):
    ```bash
    cd data-management-interview-candidate
    git fetch origin pull/<ID>/head:solution
    git checkout solution
    ```
