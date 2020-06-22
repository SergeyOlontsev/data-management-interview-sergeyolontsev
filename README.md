Process for using individual projects as "take home" assignments

1. Create a private github repository under the PulsePoint organizaiton named something like project-name-candidate-name (for example data-management-interview-mpedersen)
2. Clone this repository: `git clone git@github.com:pulsepointinc/ad-serving-interview.git`
3. Create a new directory for the candidate project: `mkdir ad-serving-interview-candidate`
4. Copy relevant projects into new directory: `cp -R ad-serving-interview/classifier/* ad-serving-interview-candidate`
5. Initialize candidate git repository
    ```bash
    cd ad-serving-interview-candidate
    git init
    git add .
    git commit -m 'initial commit'
    ```
6. Point interview project at candidate repository in github and push
    ```bash
    git remote add origin git@github.com:pulsepointinc/ad-serving-interview-candidate.git
    git push -u origin master
    ```
7. Invite candidate (via his/her github handle) and Exchange team to the new project. The candidate's access level can be made "read only", requiring the candidate to fork the project and submit a PR. The Exchange team should be added with admin rights.
8. Send an e-mail to the candidate explaining the assignment:
    ```bash
    You should get a github invite to the
    https://github.com/pulsepointinc/ad-serving-interview-candidate
    repository. There's a README there that explains what this project
    does and how to run it. There are some failing tests; the goal is to
    get the tests to pass. There are no rules or limits. We do ask that
    you fork the project then work in a branch to fix tests. When
    completed, submit a Pull Request against the master branch of the
    original repository.
    ```
9. Await a pull request, and pull it down locally, replacing <ID> below with the PR id number (typically 1 if there is only one PR against the candidate's repository):
    ```bash
    cd ad-serving-interview-candidate
    git fetch origin pull/<ID>/head:solution
    git checkout solution
    ```
10. Run tests to verify solution: `mvn test`
