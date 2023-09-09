# Tests: What are they and why?

## Contextualizing
A while ago (when I still had a computer) and I went to make a pull request for a feature here for TabNews, I couldn't run it completely, due to the lack of an automated test for the function, having had the help of Felipe Deschamps to solve.

## Tests
There are several types of software testing techniques you can use to ensure that changes to your code work as expected. However, not all testing is created equal, and we've explored how some testing practices differ.

## Automated Tests
Automated tests are performed by a machine running a pre-written test script. These tests can vary greatly in terms of complexity, ranging from checking a single method in a class to ensuring that performing a complex sequence of actions in the user interface leads to the same results. They are much more robust and reliable than manual tests. However, the quality of automated tests depends on how well your test scripts were written. If you're just getting started with testing, you can read the continuous integration tutorial to help with your first test suite.

## Some common test types
### unit tests
Unit tests are done at a very low level, close to the application's source code. They consist of testing individual methods and functions of classes, components or modules used by the software. Unit tests are generally inexpensive to automate and can be quickly executed by a continuous integration server.

### Integration tests
Integration tests verify that different modules or services used by your application work well together. For example, it could be testing database interaction or ensuring microservices work together as expected. Running these types of tests is more costly, as they require multiple parts of the application to be up and running.

### Functional tests
Functional testing focuses on the business requirements of an application. They only check the output of an action and do not check the intermediate states of the system when performing that action.
There is sometimes confusion between integration tests and functional tests, as both require multiple components to interact with each other. The difference is that an integration test might simply verify that you can query the database, whereas a functional test would expect to get a specific value from the database as defined by the product requirements.

### End-to-end testing
End-to-end testing replicates a user's behavior with the software in a complete application environment. It verifies that various user flows work as expected and can be as simple as loading a webpage or logging in or much more complex scenarios checking email notifications, online payments, etc.
End-to-end tests are very useful, but they are expensive and can be difficult to update when automated. We recommend having a few essential end-to-end tests and relying more on lower-level test types (unit and integration tests) so that you can quickly identify changes that cause failure.

### acceptance test
Acceptance tests are formal tests performed to verify that a system

### performance test
Performance tests evaluate a system's performance under a specific workload. These tests help measure an application's reliability, speed, scalability, and responsiveness. For example, performance testing can look at response times when running a large number of requests, or see how the system behaves with significant amounts of data. It can determine if an application meets performance requirements, find bottlenecks, measure stability during traffic spikes, and much more.

---

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
