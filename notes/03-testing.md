# Testing

- In most user-facing software apps, there are three types of testing to consider:
  - **Unit**: tests a small unit of code (often a function) in isolation from the rest of the system, mocking or stubbing out any dependencies.
  - **Integration**: tests 2 or more units together, verifying that they integrate with one another correctly.
  - **End to End (e2e)**: simulates a user as they would actually use the product.

<!-- break -->

## Unit Tests

- Tests that focus on one small part of the application.
- Writing unit tests forces you to consider the app architecture and break it down into small, unrelated chunks with as few dependencies as possible.
- Favours **pure** functions: for a given set of arguments, the same value should be returned, with no side effects.
- Unpredictable dependencies should be stubbed to ensure they provide predictable, canned responses.
- Unit tests are quick to run, and so should be run as often as necessary.

You should aim to write the majority of your tests as unit tests - a good rule of thumb is 70%.

<!-- break -->

## Integration Tests

- Verifies that two or more units operate correctly when integrated with one another.
- Integration tests can mock/stub dependencies, or use them as necessary.
- Integration tests will often hit HTTP endpoints or connect to a database as part of the test, making them much slower than unit tests.

It's important to verify critical parts of your application work together with integration tests. A good rule of thumb is to aim for around 20% of your tests to be integration tests.

<!-- break -->

## End to End (e2e) tests

- A good idea in theory: simulates how a real user would interact with your app.
- In practice, e2e tests are difficult to write, slow to run and can be unreliable as they depend on so many moving parts.
- Choose e2e tests for critical features in your app, and run them when you deploy as so-called 'smoke' tests.

Since e2e tests are cumbersome, aim for around 10% of your tests to be e2e ones. Choose a few critical flows, such as user sign in to test.

<!-- break -->

## The testing pyramid

``` js
                            /=\
                           /   \
                          / e2e \
                         / (10%) \
                        /=========\
                       /           \
                      / Integration \
                     /    (~20%)     \
                    /=================\
                   /                   \
                  /                     \
                 /         Unit          \
                /         (~70%)          \
               /                           \
              /=============================\
```

<!-- break -->

## How to write a unit test

- Test your **public** functions, since these are the ones that will be called by other parts of the app.
- Don't test trivial code. Stick to more complex parts, e.g. code with conditional logic.
- Don't tie to the implementation. Think of the function under test as a black box. _If I provide values x and y, will I get z?_
- To write your test:
  - _Setup_ the test by preparing the arguments to use
  - _Call_ the function with the prepared args
  - _Assert_ that the return value matches an expectation.

<!-- break -->

### Example unit test

``` js
function isAM(now) {
  return now.getHours() < 12;
}

describe('isAM', () => {
  it('should return true for times in the morning', () => {
    const sixOClock = new Date().setHours(6);
    expect(isAM(sixOClock)).toEqual(true);
    const elevenOClock = new Date().setHours(11);
    expect(isAM(elevenOClock)).toEqual(true);
  });
  it('should return false for times in the afternoon', () => {
    const twelveOClock = new Date().setHours(12);
    expect(isAM(sixOClock)).toEqual(false);
    const eightOClock = new Date().setHours(8);
    expect(isAM(eightOClock)).toEqual(false);
  });
})
```

<!-- break -->

## How to write an integration test

- Try to stub out any external dependencies wherever possible, to speed up your tests and improve their reliability.
- For example, if the test involves fetching data from an HTTP endpoint, consider mocking the endpoint and responsing with pre-canned data to compare against.
- Where this isn't possible (e.g. calling a database), try to use a local version of the dependency wherever possible, and spin it up specifically for the purposes of the test runner. This will help to minimise downtime issues.

<!-- break -->

### Example integration test

``` js
async function fetchData() {
  const resp = await fetch('http://example.com/api/data');
  return await resp.json();
}

describe('fetchData', () => {
  it('should return the data from the endpoint', () => {
    mock(fetch).andReturn({
      json: () => ({ some: 'data '})
    });
    expect(fetchData()).toEqual({ some: 'data' });
  });
  it('should throw an exception', () => {
    mock(fetch).andThrow(new Error('Network Error'));
    const tester = () => fetchData();
    expect(tester).toThrow(instanceof Error);
  });
})
```

<!-- break -->

## How to write an e2e test

- Try not to!! e2e tests are notoriously slow and flaky.
- Identify the critical parts of your application that provide the core value, and focus on those only.
- If you have a QA team, ask them to be involved with identifying and writing the English-language version of the tests.
- You can then translate these into the test code necessary to carry out the e2e tests.

<!-- break -->

## Testing tools: unit/integration

- **Jasmine** and **Mocha** have traditionally been used to unit test JavaScript apps.
- [Jest](https://facebook.github.io/jest/) is a newcomer. Backed by Facebook, and inspired by Jasmine, it has an impressive list of features:
  - Runs tests in parallel for faster overall execution time
  - Built-in code coverage reporting
  - 'Watch mode', automatically runs only recently changed files / failed tests
  - Powerful, built-in mocking library
  - Works wih TypeScript out of the box
- Since Jest is so similar to Jasmine, migrating to Jest from a suite of Jasmine tests is a less daunting task.

<!-- break -->

## Testing tools: e2e

- The incumbent is Selenium, an application which controls a brower by simulating clicks, keyboard events etc rom a script.
- It is mature but can be unwieldy and cumbersome to use.
- [TestCafe](https://github.com/DevExpress/testcafe) is a new entrant to the market - unlike Selenium, it injects JavaScript into a browser page, which means it supports almost any browser (desktop & mobile).
- If cross browser testing isn't a concern, consider [Puppeteer](https://github.com/GoogleChrome/puppeteer), a Node.js library for controlling Headless Chrome.

<!-- break -->

## Further Reading

- [The Testing Pyramid](http://www.agilenutshell.com/episodes/41-testing-pyramid)
- [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [An Overview of JavaScript Testing in 2018](https://medium.com/welldone-software/an-overview-of-javascript-testing-in-2018-f68950900bc3)
- [User Interface Testing with Jest and Puppeteer](https://www.valentinog.com/blog/ui-testing-jest-puppetteer/)
- [Just Say No to More End-to-End Tests](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html)