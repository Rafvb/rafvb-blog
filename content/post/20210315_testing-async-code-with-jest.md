+++
title = "Testing asynchronous code with jest"
description = "Setting up a new blog (again)."
tags = [
    "jest",
    "testing"
]
date = 2021-03-15
+++

Going through our codebase I've found some tests like this in our code:

```
describe('Example tests', () => {
    it('tests asynchronous code', () => {
        // Arrange code here

        // Act code here

        // Assert
        x.someAsyncMethod().subscribe((_) => {
            expect(foo).toHaveBeenCalledWith(bar);
        });
    });
});
```

These tests want to assert that something happened, but since the code is asynchronous (for example using an Observable) the test needs to subscribe to it and do the expects there.

If you run this test it will *always* pass. 😨

Because the test runner is not aware that something will happen _after_ the test completes, it will run through the code, subscribe to the async methods return value and then complete. Since there are no other expectations it will be marked as ✅.

_Side note: if this test was correctly written using the red-green-refactor TDD steps, it would not have been possible to get it in the expected red state (where foo was not called with bar) and this mistake could have been prevented_

To make sure the test waits until the async logic is finished we need to explicitly define when our test is done. To do that we can make use of the [done parameter](https://jestjs.io/docs/asynchronous#callbacks).

```
describe('Example tests', () => {
    it('tests asynchronous code', done => {
        // Arrange code here

        // Act code here

        // Assert
        x.someAsyncMethod().subscribe((_) => {
            expect(foo).toHaveBeenCalledWith(bar);
        
            done();
        });
    });
});
```

Jest will know to wait for the `done()` invocation because we request a done parameter. If it is not called jest will let the test fail after a timeout.

The test now works as expected.