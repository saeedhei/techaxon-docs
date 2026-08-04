stages:
  - test

before_script:
  - corepack enable

test-runner:
  stage: test

  tags:
    - techaxon

  script:
    - echo "Runner works!"
    - node --version
    - pnpm --version
