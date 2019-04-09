# Jest collectCoverageFrom bug report

As of jest 24 (up to 24.7.1), when configuring jest with projects where one defines `collectCoverageFrom`, it appears to be ignored.

## Setup

`yarn install`

## Reproduce coverage issue

First run jest with --coverage from the top level.  Note it does not include coverage from files that are not tested.
```
› yarn test --coverage
yarn run v1.13.0
$ jest --verbose --coverage
 PASS  packages/a/src/scripts/expected-to-be-covered-and-is.test.js
  ✓ covered is covered (6ms)

----------------------------------|----------|----------|----------|----------|-------------------|
File                              |  % Stmts | % Branch |  % Funcs |  % Lines | Uncovered Line #s |
----------------------------------|----------|----------|----------|----------|-------------------|
All files                         |      100 |      100 |      100 |      100 |                   |
 expected-to-be-covered-and-is.js |      100 |      100 |      100 |      100 |                   |
----------------------------------|----------|----------|----------|----------|-------------------|
```

Now test the package directly.  `collectCoverageFrom` is acting as expected, it reports that `expected-to-be-covered-but-isnt.js` isn't covered.

```
› cd packages/a
› yarn test --coverage
yarn run v1.13.0
$ jest --verbose --coverage
 PASS  src/scripts/expected-to-be-covered-and-is.test.js
  ✓ covered is covered (5ms)

------------------------------------|----------|----------|----------|----------|-------------------|
File                                |  % Stmts | % Branch |  % Funcs |  % Lines | Uncovered Line #s |
------------------------------------|----------|----------|----------|----------|-------------------|
All files                           |       50 |      100 |      100 |       50 |                   |
 expected-to-be-covered-and-is.js   |      100 |      100 |      100 |      100 |                   |
 expected-to-be-covered-but-isnt.js |        0 |      100 |      100 |        0 |                 1 |
------------------------------------|----------|----------|----------|----------|-------------------|
```
