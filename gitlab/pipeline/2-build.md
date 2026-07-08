stages:
  - build

before_script:
  - corepack enable

build:
  stage: build

  tags:
    - techaxon

  script:
    - pnpm install --frozen-lockfile
    - pnpm build
