---
layout: post
title:  "Enter your title here"
date:   2017-04-12 04:18:54 +0000
---


Testing is an essential part of the software development style. While writing tests is great for debugging code and maintaining it over time, one of the biggest value adds is actually providing a guideline for how the code is actually written. By reading and writing tests, developers have clarity on what they are building. This type of design process is known as Test Driven Development.

Test Driven Development (TDD) was introduced/rediscovered in 2003 by Kent Beck, the creator of extreme programming (a software development methodology) and one of the 17 signatories of the Agile Manifesto. TDD is, simply put, a software design process in which writing tests comes before writing the production code. 

The general thought process of TDD is:

1. write a test that fails
2. write code to pass the test
3. refactor the code to make it right

Another term for this process is commonly referred to as “Red, Green, Refactor”

1. Red: write a test that doesn’t work
2. Green: pass the test with code that works, but is not necessarily clean
3. Refactor: clean the code

This type of development process differs dramatically from something like architecture-driven design, where clean code is written first and then other parts of the “unit” are integrated after to solve the “does it work” problem.

While the concept of TDD may be simple to understand, the implementation can be more difficult. Implementing any testing style can be difficult simply due the variety of testing terminology that exists today and the misconceptions that arise between each type of testing. Take a look at http://www.aptest.com/testtypes.html for example, there a 14 different types of tests described, of which, some are scoped inside of another. The best way to think about these different types of testing is that some of these testing types are written from the perspective of a developer and while others are written from the perspective of a customer or end user. 

From a developer's perspective of using TDD to write code, what makes TDD difficult to implement is how to think about each “unit” to test and how to write tests that test the “integration" of these smaller units. TDD requires at a minimum two styles of tests to successfully test an application, Unit Tests and Integration Tests.

At the most minute level, tests need to written that test specific functions in a program. These are commonly referred to as Unit Tests. A Unit test should:

* be quick to run
* be independent from other tests
* have the minimum number of assertions, only test one or as few expectations as possible
* avoid many pre-conditions
* check the results of the function, not the inner workings
* uses clear descriptions to define what the test is doing

When writing unit tests, it is best to go in order of answering the following questions: 
1. What are you testing?
2. What should it do?
3. What is the expected output?
4. Does your actual output meet the expected output?
5. Do you need any pre-conditional information to run the test?

It may be confusing that adding any pre-conditional information is the last step, but when thinking about writing the test, it is easier to start with what your are comparing and then add in the pre-conditional information based on that. 

Let say we want to write a unit test for checkPassword(). This example is written in Mocha, a testing suite for Javascript:

describe(‘checkPassword', () => {
  it(’should return false if the password is less than  6 digits', () => {
    const Result = checkPassword(‘ab123’);
    const expectedResult = false;
    expect(falseResult).toBe(expectedResult);
  });

  it(’should return false if the password contains any script tag', () => {
    const result = checkPassword(‘<s>hack<s>');
    const expectedResult = false;
    expect(result).toBe(expectedResult);
  });
});

The next step up from unit testing is Integration Testing. Integration Testing looks to see if each of these unit tests work together to properly perform a component of the application. When running integration test, the assumption is that the unit tests for each piece of a component being tested is already working. In an large application, integration testing can be expanded out to multiple levels, meaning that integration tests are test for each component, and integration tests can then be performed on top of these components. The point of an integration test in TDD is to guide the developer to connecting independent units, whether they be components or individual functions.

Lets take the original unit test a step further and create an integration test for the Sign Up Page. You will notice that the Integration typically can involve routes and accessing the database. It is common that many integration tests will include stubs and mocks, however the example below does not. The example below still uses Mocha, but this time includes the Chai Assertion Library with the Jquery Plugin.

var request = require("request"),
var assert = require('assert’),
var app = require("../app.js"),
var base_url = "http://localhost:3000/signup";

describe(“sign up page", function() {
  describe("GET /signup", function() {
    it("returns status code 200", function(done) {
      request.get(base_url, function(error, response, body) {
        assert.equal(200, response.statusCode);
        done();
      });
    });

    it("returns signup form", function(done) {
      request.get(base_url, function(error, response, body) {
        expect($('body')).to.have.html($(‘form.signup’));
        app.closeServer();
        done();
      });
    });
  });

  describe(“POST /signup", function() {
     it(“add as user to the database if password is valid", function(done) {
     //asserts that session id is set when User.signup is called
        done();
      });
    });
    it(“redirects to login page", function(done) {
      //asserts that body has login form
      });
    });

…

This integration test is testing the routes as well as the methods that are required to perform the actions of the signing up. Notice that we are not checking if specific methods work, but rather they are working together.

In case your still a little fuzzy about why TDD involves unit tests as well as integration tests, see below:

<iframe src='https://gfycat.com/ifr/HotOrangeCoypu' frameborder='0' scrolling='no' width='640' height='640' allowfullscreen></iframe>
https://gfycat.com/HotOrangeCoypu


