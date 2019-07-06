# 102 Seeding Database with test data
* We will learn how to manipulate our test Database before our test cases run
* We saw we had a problem when testing the creation of a new user with createUser
    - You create a user
    - You can't test again because that user was created and that email was taken and we got the "email constraint" error

## Jest gives us life cycle tools
* API > API Reference > Globals
* [api docs](https://jestjs.io/docs/en/api)

### after and before methods
* afterAll(fn, timeout)
    - Will run a single time after all tests are done
* afterEach(fn, timeout)
    - Will run after each test case
* beforeAll(fn, timeout)
    - Will run before all tests are run
* beforeEach(fn, timeout)
    - Will run before each test case

#### beforeEach()
* We will use this to run before to clear all records so all tests start with the same test Database