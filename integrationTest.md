# Game Service - Integration test guide

#### Install packages

```sh
# Install sc-game-service-integration
yarn add sc-game-service-integration

# Install jest-junit
yarn add jest-junit --dev
```

#### Configuration

-   In **package.json** add the following testResultsProcessor entry:

```sh
"jest": {
    "testResultsProcessor": "jest-junit"
}
```

-   In your jest config add sc-game-service-integration to transformIgnorePatterns:

```sh
transformIgnorePatterns: [
    "node_modules/(?!(sc-base-database|sc-common|sc-game-base|sc-base-apis|sc-api-client|sc-game-service-integration)/)",
],
```

#### Prepare the test data & test data setup

-   Test data (depends on business of service you need to test)
    https://git3.nexdev.net/l1/game-services/sedie-service/-/blob/integration-test/__integration-tests__/stubs/test-data.js

-   Implement a job that removes all data before running test, then creates the test data
    https://git3.nexdev.net/l1/game-services/sedie-service/-/blob/integration-test/__integration-tests__/stubs/test-data-setup.js

#### Prepare test service

-   Implement container registry that injects stub objects
    https://git3.nexdev.net/l1/game-services/sedie-service/-/blob/integration-test/__integration-tests__/game-service-setup/container-registry.js

-   Implement game service app that uses the container registry
    https://git3.nexdev.net/l1/game-services/sedie-service/-/blob/integration-test/__integration-tests__/game-service-setup/game-service-app.js

-   Add config transform for integration test
    https://git3.nexdev.net/l1/game-services/sedie-service/-/blob/integration-test/configs/app-configs.IGT.js

-   Add integration test commands to **package.json**:

```sh
"test-:-integration-:-setup-test-data": "node --es-module-specifier-resolution=node ./__integration-tests__/stubs/test-data-setup.js",
"test-:-integration-:-start-service": "node --es-module-specifier-resolution=node ./__integration-tests__/game-service-setup/game-service-app.js",
"test-:-integration-:-start-test": "jest --testPathPattern=__integration-tests__/  --detectOpenHandles --ci --reporters=default --reporters=jest-junit",
```

#### add integration step to gitlab.ci

```sh
integration-tests:
  stage: test
  needs: ['unit-test-js']
  tags:
    - docker

  services:
    - name: registry.nexdev.net:5050/docker/database/mongo:4.4.8
      alias: mongo

  image: registry.nexdev.net:5050/docker/node:14.15.1-slim
  before_script:
    - cp ./src/app-configs.js ./src/app-configs.original.js
    - cp ./configs/app-configs.IGT.js ./src/app-configs.js
    - npm set registry http://npm.lumigame.com
    - yarn install
    - npm run test-:-integration-:-setup-test-data
    - nohup npm run test-:-integration-:-start-service &

  script:
    - npm run test-:-integration-:-start-test

  artifacts:
    when: always
    paths:
      - ./coverage
    reports:
      junit:
        - junit.xml
    expire_in: 1 day
```
