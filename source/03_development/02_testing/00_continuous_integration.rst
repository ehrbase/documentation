CI/CD
=====

This part of the documentation describes how continuous integration helps us to have a fast feedback loop during development. This allows the project to keep up testing with the fast pace of iterations within the agile development environment (Scrum sprints).




Continuous Integration
----------------------

EHRbase uses `CircleCI <https://app.circleci.com/pipelines/github/ehrbase>`_ for continuous integration and deployment. The CI pipeline consists of the following workflows:

Pipeline workflow 1/3 - build-and-test
--------------------------------------

- trigger: commit to any branch (except - release/v*, master, sync/*, feature/sync/*)
- jobs:
    - build artifacts
    - run unit tests
    - run sdk integraiton tests
    - run robot integration tests
    - perform sonarcloud analysis and OWASP dependency check


Pipeline workflow 2/3 - release
-------------------------------

- trigger: commit to release/v or master branch
- jobs:
    - build artifacts
    - run unit tests
    - run sdk integraiton tests
    - run robot integration tests
    - perform sonarcloud analysis and OWASP dependency check
    - TODO: deploy to Maven Central
    - TODO: deploy to Docker Hub


Pipeline workflow 3/3 - synced-feature-check
--------------------------------------------

⚠️ This is a special workflow to catch errors that can occur when code changes introduced to EHRbase AND openEHR_SDK repository are related in a way that they have to be tested together and otherwise can't be catched in workflow 1 or 2.

- trigger: commit to sync/* branch
- jobs:
    - pull, build, and test SDK from sync/* branch of openEHR_SDK repo
    - build and test ehrbase (w/ SDK installed in previous step)
    - start ehrbase server (from .jar packaged in previous step)
    - run SDK's (java) integration tests
    - run EHRbase's (robot) integration tests

.. code-block:: bash

    HOW TO USE WORKFLOW 3/3
    =======================

    1. create TWO branches following the naming convention `sync/[issue-id]_some-desciption`
       in both repositories (EHRbase and openEHR_SDK) with exact the same name:

      - ehrbase repo       --> i.e.    sync/123_example-issue
      - openehr_sdk repo   --> i.e.    sync/123_example-issue

    2. apply your code changes
    3. push to openehr_sdk repo (NO CI will be triggered)
    4. push to ehrbase repo (CI will trigger this workflow)
    5. create TWO PRs (one in EHRbase, one in openEHR_SDK)
    6. merge BOTH PRs considering below notes:
      - make sure both PRs are reviewed and ready to be merged
        at the same time!
      - make sure to sync both PRs w/ develop before merging!
      - MERGE BOTH PRs AT THE SAME TIME!
