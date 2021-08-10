# Testing

## Functional testing

- Functional testing is a type of testing which verifies that each function of the software application operates in conformance with the requirement specification.
- Every functionality of the system is tested by providing appropriate input, verifying the output and comparing the actual results with the expected results.
- The testing can be done either manually or using automation.

## Non-functional testing

- Non-functional testing verifies non-functional aspects like performance, usability, reliability, etc.
- A good example of non-functional test would be to check how many people can simultaneously login into a software.
- The testing is done mostly using automation because it is hard to do manually.

> A Functional Testing example is to check the login functionality whereas a Non Functional testing example is to check the dashboard should load in 2 seconds.

> Functional describes what the product does whereas Non Functional describes how the product works.

## Types of functional testing

### Alpha testing

The objective of this testing is to identify all possible issues or defects before releasing it into the market or to the user. Alpha Testing is carried out at the end of the software development phase but before the Beta Testing. Still, minor design changes may be made as a result of such testing. Alpha Testing is conducted at the developer’s site. In-house virtual user environment can be created for this type of testing.

### Beta/Acceptance testing

Beta Testing is a formal type of Software Testing which is carried out by the customer. It is performed in the Real Environment before releasing the product to the market for the actual end-users. Usually, this testing is typically done by end-users or others. It is the final testing done before releasing an application for commercial purpose. Usually, the Beta version of the software or product released is limited to a certain number of users in a specific area.

### Accessibility testing

The aim of Accessibility Testing is to determine whether the software or application is accessible for disabled people or not. Here, disability means deaf, color blind, mentally disabled, blind, old age and other disabled groups.

### Backward compatibility testing

It is a type of testing which validates whether the newly developed software or updated software works well with the older version of the environment or not.

### Boundary value testing

Boundary Value Testing is performed for checking if defects exist at boundary values. There is an upper and lower boundary for each range and testing is performed on these boundary values. If testing requires a test range of numbers from 1 to 500 then Boundary Value Testing is performed on values at 0, 1, 2, 499, 500 and 501.

### Integration/Component testing

Multiple pieces are tested together. It is mostly performed by developers after the completion of unit testing. Component Testing involves testing of multiple functionalities as a single code and its objective is to identify if any defect exists after connecting those multiple functionalities with each other.

### End-to-End testing

Similar to system testing, End-to-End Testing involves testing of a complete application environment in a situation that mimics real-world use.

### Regression testing

Type of software testing to confirm that a recent program or code change has not adversely affected existing features. Regression Testing is nothing but a full or partial selection of already executed test cases which are re-executed to ensure existing functionalities work fine.

### Unit Testing

A unit test focuses on a single “unit of code” – usually a function in an object or module. By making the test specific to a single function, the test should be simple, quick to write, and quick to run. This means you can have many unit tests, and more unit tests means more bugs caught. They are especially helpful if you need to change your code: When you have a set of unit tests verifying your code works, you can safely change the code and trust that other parts of your program will not break.

A unit test should be isolated from dependencies – for example, no network access and no database access. There are tools that can replace these dependencies with fakes you can control. This makes it trivial to test all kinds of scenarios that would otherwise require a lot of setup. For example, imagine having to set up an entire database just to run a test. Ugh, no thanks.

The basic pieces of a unit test are there: Individual tests, which test one thing, and they are isolated from each other. You could use scripts like this to build rudimentary unit tests for your code. But using an actual unit testing tool such as Mocha or Jasmine will make it easier to write tests, and they have other helpful features such as better reporting when tests fail (which makes it easier to find out what went wrong)

## Types of non-functional testing

### Load testing

It is a type of Non-Functional Testing and the objective of Load Testing is to check how much load or maximum workload a system can handle without any performance degradation.

### Monkey testing

Monkey Testing is carried out by a tester assuming that if the monkey uses the application then how random input, values will be entered by the Monkey without any knowledge or understanding of the application. The objective of Monkey Testing is to check if an application or system gets crashed by providing random input values/data.

### Performance testing

This term is often used interchangeably with ‘stress’ and ‘load’ testing. Performance Testing is done to check whether the system meets the performance requirements.

### Recovery testing

It is a type of testing which validates how well the application or system recovers from crashes or disasters. Recovery Testing determines if the system is able to continue the operation after a disaster. Assume that application is receiving data through the network cable and suddenly that network cable has been unplugged. Sometime later, plug the network cable; then the system should start receiving data from where it lost the connection due to network cable unplugged.

### Security testing

Security Testing is done to check how the software or application or website is secure from internal and external threats. This testing includes how much software is secure from the malicious program, viruses and how secure and strong the authorization and authentication processes are.

### Usability testing

Under Usability Testing, User-friendliness check is done. The application flow is tested to know if a new user can understand the application easily or not, Proper help documented if a user gets stuck at any point.

### Volume testing

Volume Testing is a type of Non-Functional Testing performed by the Performance Testing team. The software or application undergoes a huge amount of data and Volume Testing checks the system behavior and response time of the application when the system came across such a high volume of data. This high volume of data may impact the system’s performance and speed of the processing time.
