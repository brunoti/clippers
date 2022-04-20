# javascript-testing-best-practices/readme.md at master · goldbergyoni/javascript-testing-best-practices
[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/jtbp-header-blue.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/jtbp-header-blue.png)

## 🎊 April 2022 Announcement: A new edition was just released with 5 new best practices, many more code examples and 4 new language translations

## 👨‍🏫 Next workshop: Verona, Italy 🇮🇹, April 20th. [Tickets and more info here](https://2022.jsday.it/workshop/nodejs_testing.html)

## 📗 50+ best practices: Super-comprehensive and exhaustive

This is a guide for JavaScript & Node.js reliability from A-Z. It summarizes and curates for you dozens of the best blog posts, books and tools the market has to offer

## 🚢 Advanced: Goes 10,000 miles beyond the basics

Hop into a journey that travels way beyond the basics into advanced topics like testing in production, mutation testing, property-based testing and many other strategic & professional tools. Should you read every word in this guide your testing skills are likely to go way above the average

## 🌐 Full-stack: front, backend, CI, anything

Start by understanding the ubiquitous testing practices that are the foundation for any application tier. Then, delve into your area of choice: frontend/UI, backend, CI or maybe all of them?

### Written By Yoni Goldberg

-   A JavaScript & Node.js consultant
-   📗 [Testing Node.js & JavaScript From A To Z](https://www.testjavascript.com/) - My comprehensive online course with more than [7 hours of video](https://www.testjavascript.com/), 14 test types and more than 40 best practices
-   [Follow me on Twitter](https://twitter.com/goldbergyoni/)
-   [Next workshop: Verona, Italy 🇮🇹, April 20th](https://2022.jsday.it/workshop/nodejs_testing.html)

### Translations - read in your own language

-   🇨🇳[Chinese](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-zh-CN.md) - Courtesy of [Yves yao](https://github.com/yvesyao)
-   🇰🇷[Korean](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme.kr.md) - Courtesy of [Rain Byun](https://github.com/ragubyun)
-   🇵🇱[Polish](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-pl.md) - Courtesy of [Michal Biesiada](https://github.com/mbiesiad)
-   🇪🇸[Spanish](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-es.md) - Courtesy of [Miguel G. Sanguino](https://github.com/sanguino)
-   🇧🇷[Portuguese-BR](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-pt-br.md) - Courtesy of [Iago Angelim Costa Cavalcante](https://github.com/iagocavalcante) , [Douglas Mariano Valero](https://github.com/DouglasMV) and [koooge](https://github.com/koooge)
-   🇫🇷[French](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-fr.md) - Courtesy of [Mathilde El Mouktafi](https://github.com/mel-mouk)
-   🇯🇵[Japanese (draft)](https://github.com/yuichkun/javascript-testing-best-practices/blob/master/readme-jp.md) - Courtesy of [Yuichi Yogo](https://github.com/yuichkun) and [ryo](https://github.com/kawamataryo)
-   🇹🇼[Traditional Chinese](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-zh-TW.md) - Courtesy of [Yubin Hsu](https://github.com/yubinTW)
-   Want to translate to your own language? please open an issue 💜

## `Table of Contents`

#### [`Section 0: The Golden Rule`](#section-0%EF%B8%8F%E2%83%A3-the-golden-rule)

A single advice that inspires all the others (1 special bullet)

#### [`Section 1: The Test Anatomy`](#section-1-the-test-anatomy-1)

The foundation - structuring clean tests (12 bullets)

#### [`Section 2: Backend`](#section-2%EF%B8%8F%E2%83%A3-backend-testing)

Writing backend and Microservices tests efficiently (13 bullets)

#### [`Section 3: Frontend`](#section-3%EF%B8%8F%E2%83%A3-frontend-testing)

Writing tests for web UI including component and E2E tests (11 bullets)

#### [`Section 4: Measuring Tests Effectiveness`](#section-4%EF%B8%8F%E2%83%A3-measuring-test-effectiveness)

Watching the watchman - measuring test quality (4 bullets)

#### [`Section 5: Continuous Integration`](#section-5%EF%B8%8F%E2%83%A3-ci-and-other-quality-measures)

Guidelines for CI in the JS world (9 bullets)

## ⚪️ 0 The Golden Rule: Design for lean testing

✅ **Do:** Testing code is not production-code - Design it to be short, dead-simple, flat, and delightful to work with. One should look at a test and get the intent instantly.

See, our minds are already occupied with our main job - the production code. There is no 'headspace' for additional complexity. Should we try to squeeze yet another sus-system into our poor brain it will slow the team down which works against the reason we do testing. Practically this is where many teams just abandon testing.

The tests are an opportunity for something else - a friendly assistant, co-pilot, that delivers great value for a small investment. Science tells us that we have two brain systems: system 1 is used for effortless activities like driving a car on an empty road and system 2 which is meant for complex and conscious operations like solving a math equation. Design your test for system 1, when looking at test code it should _feel_ as easy as modifying an HTML document and not like solving 2X(17 × 24).

This can be achieved by selectively cherry-picking techniques, tools and test targets that are cost-effective and provide great ROI. Test only as much as needed, strive to keep it nimble, sometimes it's even worth dropping some tests and trade reliability for agility and simplicity.

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/headspace.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/headspace.png)

Most of the advice below are derivatives of this principle.

### Ready to start?

## ⚪ ️ 1.1 Include 3 parts in each test name

✅ **Do:** A test report should tell whether the current application revision satisfies the requirements for the people who are not necessarily familiar with the code: the tester, the DevOps engineer who is deploying and the future you two years from now. This can be achieved best if the tests speak at the requirements level and include 3 parts:

(1) What is being tested? For example, the ProductsService.addNewProduct method

(2) Under what circumstances and scenario? For example, no price is passed to the method

(3) What is the expected result? For example, the new product is not approved

❌ **Otherwise:** A deployment just failed, a test named “Add product” failed. Does this tell you what exactly is malfunctioning?

**👇 Note:** Each bullet has code examples and sometime also an image illustration. Click to expand  

✏ **Code Examples**  

### 👏 Doing It Right Example: A test name that constitutes 3 parts

[![](https://camo.githubusercontent.com/39cc1d33840f29d375f3690dfe2ec61a78e5c08534bdf88da7de17e7a4fad0f8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/39cc1d33840f29d375f3690dfe2ec61a78e5c08534bdf88da7de17e7a4fad0f8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

```js
//1. unit under test
describe('Products Service', function() {
  describe('Add new product', function() {
    //2. scenario and 3. expectation
    it('When no price is specified, then the product status is pending approval', ()=> {
      const newProduct = new ProductService().add(...);
      expect(newProduct.status).to.equal('pendingApproval');
    });
  });
});
```

### 👏 Doing It Right Example: A test name that constitutes 3 parts

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-1-3-parts.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-1-3-parts.jpeg)

© **Credits & read-more** 1. [Roy Osherove - Naming standards for unit tests](https://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html)

## ⚪ ️ 1.2 Structure tests by the AAA pattern

✅ **Do:** Structure your tests with 3 well-separated sections Arrange, Act & Assert (AAA). Following this structure guarantees that the reader spends no brain-CPU on understanding the test plan:

1st A - Arrange: All the setup code to bring the system to the scenario the test aims to simulate. This might include instantiating the unit under test constructor, adding DB records, mocking/stubbing on objects and any other preparation code

2nd A - Act: Execute the unit under test. Usually 1 line of code

3rd A - Assert: Ensure that the received value satisfies the expectation. Usually 1 line of code

❌ **Otherwise:** Not only do you spend hours understanding the main code, but what should have been the simplest part of the day (testing) stretches your brain

✏ **Code Examples**  

### 👏 Doing It Right Example: A test structured with the AAA pattern

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667) [![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

```js
describe("Customer classifier", () => {
  test("When customer spent more than 500$, should be classified as premium", () => {
    //Arrange
    const customerToClassify = { spent: 505, joined: new Date(), id: 1 };
    const DBStub = sinon.stub(dataAccess, "getCustomer").reply({ id: 1, classification: "regular" });

    //Act
    const receivedClassification = customerClassifier.classifyCustomer(customerToClassify);

    //Assert
    expect(receivedClassification).toMatch("premium");
  });
});
```

### 👎 Anti-Pattern Example: No separation, one bulk, harder to interpret

```js
test("Should be classified as premium", () => {
  const customerToClassify = { spent: 505, joined: new Date(), id: 1 };
  const DBStub = sinon.stub(dataAccess, "getCustomer").reply({ id: 1, classification: "regular" });
  const receivedClassification = customerClassifier.classifyCustomer(customerToClassify);
  expect(receivedClassification).toMatch("premium");
});
```

## ⚪ ️1.3 Describe expectations in a product language: use BDD-style assertions

✅ **Do:** Coding your tests in a declarative-style allows the reader to get the grab instantly without spending even a single brain-CPU cycle. When you write imperative code that is packed with conditional logic, the reader is forced to exert more brain-CPU cycles. In that case, code the expectation in a human-like language, declarative BDD style using `expect` or `should` and not using custom code. If Chai & Jest doesn't include the desired assertion and it’s highly repeatable, consider [extending Jest matcher (Jest)](https://jestjs.io/docs/en/expect#expectextendmatchers) or writing a [custom Chai plugin](https://www.chaijs.com/guide/plugins/)  

❌ **Otherwise:** The team will write less tests and decorate the annoying ones with .skip()

✏ **Code Examples**

[![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667) [![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

### 👎 Anti-Pattern Example: The reader must skim through not so short, and imperative code just to get the test story

```js
test("When asking for an admin, ensure only ordered admins in results", () => {
  //assuming we've added here two admins "admin1", "admin2" and "user1"
  const allAdmins = getUsers({ adminOnly: true });

  let admin1Found,
    adming2Found = false;

  allAdmins.forEach(aSingleUser => {
    if (aSingleUser === "user1") {
      assert.notEqual(aSingleUser, "user1", "A user was found and not admin");
    }
    if (aSingleUser === "admin1") {
      admin1Found = true;
    }
    if (aSingleUser === "admin2") {
      admin2Found = true;
    }
  });

  if (!admin1Found || !admin2Found) {
    throw new Error("Not all admins were returned");
  }
});
```

### 👏 Doing It Right Example: Skimming through the following declarative test is a breeze

```js
it("When asking for an admin, ensure only ordered admins in results", () => {
  //assuming we've added here two admins
  const allAdmins = getUsers({ adminOnly: true });

  expect(allAdmins)
    .to.include.ordered.members(["admin1", "admin2"])
    .but.not.include.ordered.members(["user1"]);
});
```

## ⚪ ️ 1.4 Stick to black-box testing: Test only public methods

✅ **Do:** Testing the internals brings huge overhead for almost nothing. If your code/API delivers the right results, should you really invest your next 3 hours in testing HOW it worked internally and then maintain these fragile tests? Whenever a public behavior is checked, the private implementation is also implicitly tested and your tests will break only if there is a certain problem (e.g. wrong output). This approach is also referred to as `behavioral testing`. On the other side, should you test the internals (white box approach) — your focus shifts from planning the component outcome to nitty-gritty details and your test might break because of minor code refactors although the results are fine - this dramatically increases the maintenance burden  

❌ **Otherwise:** Your tests behave like the [boy who cried wolf](https://en.wikipedia.org/wiki/The_Boy_Who_Cried_Wolf): shouting false-positive cries (e.g., A test fails because a private variable name was changed). Unsurprisingly, people will soon start to ignore the CI notifications until someday, a real bug gets ignored…

✏ **Code Examples**  

### 👎 Anti-Pattern Example: A test case is testing the internals for no good reason

[![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

```js
class ProductService {
  //this method is only used internally
  //Change this name will make the tests fail
  calculateVATAdd(priceWithoutVAT) {
    return { finalPrice: priceWithoutVAT * 1.2 };
    //Change the result format or key name above will make the tests fail
  }
  //public method
  getPrice(productId) {
    const desiredProduct = DB.getProduct(productId);
    finalPrice = this.calculateVATAdd(desiredProduct.price).finalPrice;
    return finalPrice;
  }
}

it("White-box test: When the internal methods get 0 vat, it return 0 response", async () => {
  //There's no requirement to allow users to calculate the VAT, only show the final price. Nevertheless we falsely insist here to test the class internals
  expect(new ProductService().calculateVATAdd(0).finalPrice).to.equal(0);
});
```

## ⚪ ️ ️1.5 Choose the right test doubles: Avoid mocks in favor of stubs and spies

✅ **Do:** Test doubles are a necessary evil because they are coupled to the application internals, yet some provide immense value ([](https://martinfowler.com/articles/mocksArentStubs.html)[Read here a reminder about test doubles: mocks vs stubs vs spies](https://martinfowler.com/articles/mocksArentStubs.html)).

Before using test doubles, ask a very simple question: Do I use it to test functionality that appears, or could appear, in the requirements document? If no, it’s a white-box testing smell.

For example, if you want to test that your app behaves reasonably when the payment service is down, you might stub the payment service and trigger some ‘No Response’ return to ensure that the unit under test returns the right value. This checks our application behavior/response/outcome under certain scenarios. You might also use a spy to assert that an email was sent when that service is down — this is again a behavioral check which is likely to appear in a requirements doc (“Send an email if payment couldn’t be saved”). On the flip side, if you mock the Payment service and ensure that it was called with the right JavaScript types — then your test is focused on internal things that have nothing to do with the application functionality and are likely to change frequently  

❌ **Otherwise:** Any refactoring of code mandates searching for all the mocks in the code and updating accordingly. Tests become a burden rather than a helpful friend

✏ **Code Examples**  

### 👎 Anti-pattern example: Mocks focus on the internals

[![](https://camo.githubusercontent.com/63ace1702903555ec43954f2f09636d1d6494a209198a8715331964d902f5767/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323053696e6f6e2d626c75652e737667)
](https://camo.githubusercontent.com/63ace1702903555ec43954f2f09636d1d6494a209198a8715331964d902f5767/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323053696e6f6e2d626c75652e737667)

```js
it("When a valid product is about to be deleted, ensure data access DAL was called once, with the right product and right config", async () => {
  //Assume we already added a product
  const dataAccessMock = sinon.mock(DAL);
  //hmmm BAD: testing the internals is actually our main goal here, not just a side-effect
  dataAccessMock
    .expects("deleteProduct")
    .once()
    .withArgs(DBConfig, theProductWeJustAdded, true, false);
  new ProductService().deletePrice(theProductWeJustAdded);
  dataAccessMock.verify();
});
```

### 👏Doing It Right Example: spies are focused on testing the requirements but as a side-effect are unavoidably touching to the internals

```js
it("When a valid product is about to be deleted, ensure an email is sent", async () => {
  //Assume we already added here a product
  const spy = sinon.spy(Emailer.prototype, "sendEmail");
  new ProductService().deletePrice(theProductWeJustAdded);
  //hmmm OK: we deal with internals? Yes, but as a side effect of testing the requirements (sending an email)
  expect(spy.calledOnce).to.be.true;
});
```

## 📗 Want to learn all these practices with live video?

### Visit my online course [Testing Node.js & JavaScript From A To Z](https://www.testjavascript.com/)

## ⚪ ️1.6 Don’t “foo”, use realistic input data

✅ **Do:** Often production bugs are revealed under some very specific and surprising input — the more realistic the test input is, the greater the chances are to catch bugs early. Use dedicated libraries like [Chance](https://github.com/chancejs/chancejs) or [Faker](https://www.npmjs.com/package/faker) to generate pseudo-real data that resembles the variety and form of production data. For example, such libraries can generate realistic phone numbers, usernames, credit card, company names, and even ‘lorem ipsum’ text. You may also create some tests (on top of unit tests, not as a replacement) that randomize fakers data to stretch your unit under test or even import real data from your production environment. Want to take it to the next level? See the next bullet (property-based testing).  

❌ **Otherwise:** All your development testing will falsely show green when you use synthetic inputs like “Foo”, but then production might turn red when a hacker passes-in a nasty string like “@3e2ddsf . ##’ 1 fdsfds . fds432 AAAA”

✏ **Code Examples**  

### 👎 Anti-Pattern Example: A test suite that passes due to non-realistic data

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
const addProduct = (name, price) => {
  const productNameRegexNoSpace = /^\S*$/; //no white-space allowed

  if (!productNameRegexNoSpace.test(name)) return false; //this path never reached due to dull input

  //some logic here
  return true;
};

test("Wrong: When adding new product with valid properties, get successful confirmation", async () => {
  //The string "Foo" which is used in all tests never triggers a false result
  const addProductResult = addProduct("Foo", 5);
  expect(addProductResult).toBe(true);
  //Positive-false: the operation succeeded because we never tried with long
  //product name including spaces
});
```

### 👏Doing It Right Example: Randomizing realistic input

```js
it("Better: When adding new valid product, get successful confirmation", async () => {
  const addProductResult = addProduct(faker.commerce.productName(), faker.random.number());
  //Generated random input: {'Sleek Cotton Computer',  85481}
  expect(addProductResult).to.be.true;
  //Test failed, the random input triggered some path we never planned for.
  //We discovered a bug early!
});
```

## ⚪ ️ 1.7 Test many input combinations using Property-based testing

✅ **Do:** Typically we choose a few input samples for each test. Even when the input format resembles real-world data (see bullet [‘Don’t foo’](https://github.com/goldbergyoni/javascript-testing-best-practices#-%EF%B8%8F16-dont-foo-use-realistic-input-data)), we cover only a few input combinations (method(‘’, true, 1), method(“string” , false , 0)), However, in production, an API that is called with 5 parameters can be invoked with thousands of different permutations, one of them might render our process down ([see Fuzz Testing](https://en.wikipedia.org/wiki/Fuzzing)). What if you could write a single test that sends 1000 permutations of different inputs automatically and catches for which input our code fails to return the right response? Property-based testing is a technique that does exactly that: by sending all the possible input combinations to your unit under test it increases the serendipity of finding a bug. For example, given a method — addNewProduct(id, name, isDiscount) — the supporting libraries will call this method with many combinations of (number, string, boolean) like (1, “iPhone”, false), (2, “Galaxy”, true). You can run property-based testing using your favorite test runner (Mocha, Jest, etc) using libraries like [js-verify](https://github.com/jsverify/jsverify) or [testcheck](https://github.com/leebyron/testcheck-js) (much better documentation). Update: Nicolas Dubien suggests in the comments below to [checkout fast-check](https://github.com/dubzzz/fast-check#readme) which seems to offer some additional features and also to be actively maintained  

❌ **Otherwise:** Unconsciously, you choose the test inputs that cover only code paths that work well. Unfortunately, this decreases the efficiency of testing as a vehicle to expose bugs

✏ **Code Examples**  

### 👏 Doing It Right Example: Testing many input permutations with “fast-check”

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
import fc from "fast-check";

describe("Product service", () => {
  describe("Adding new", () => {
    //this will run 100 times with different random properties
    it("Add new product with random yet valid properties, always successful", () =>
      fc.assert(
        fc.property(fc.integer(), fc.string(), (id, name) => {
          expect(addNewProduct(id, name).status).toEqual("approved");
        })
      ));
  });
});
```

## ⚪ ️ 1.8 If needed, use only short & inline snapshots

✅ **Do:** When there is a need for [snapshot testing](https://jestjs.io/docs/en/snapshot-testing), use only short and focused snapshots (i.e. 3-7 lines) that are included as part of the test ([Inline Snapshot](https://jestjs.io/docs/en/snapshot-testing#inline-snapshots)) and not within external files. Keeping this guideline will ensure your tests remain self-explanatory and less fragile.

On the other hand, ‘classic snapshots’ tutorials and tools encourage to store big files (e.g. component rendering markup, API JSON result) over some external medium and ensure each time when the test run to compare the received result with the saved version. This, for example, can implicitly couple our test to 1000 lines with 3000 data values that the test writer never read and reasoned about. Why is this wrong? By doing so, there are 1000 reasons for your test to fail - it’s enough for a single line to change for the snapshot to get invalid and this is likely to happen a lot. How frequently? for every space, comment or minor CSS/HTML change. Not only this, the test name wouldn’t give a clue about the failure as it just checks that 1000 lines didn’t change, also it encourages to the test writer to accept as the desired true a long document he couldn’t inspect and verify. All of these are symptoms of obscure and eager test that is not focused and aims to achieve too much

It’s worth noting that there are few cases where long & external snapshots are acceptable - when asserting on schema and not data (extracting out values and focusing on fields) or when the received document rarely changes  

❌ **Otherwise:** A UI test fails. The code seems right, the screen renders perfect pixels, what happened? your snapshot testing just found a difference from the origin document to current received one - a single space character was added to the markdown...

✏ **Code Examples**  

### 👎 Anti-Pattern Example: Coupling our test to unseen 2000 lines of code

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
it("TestJavaScript.com is renderd correctly", () => {
  //Arrange

  //Act
  const receivedPage = renderer
    .create(<DisplayPage page="http://www.testjavascript.com"> Test JavaScript </DisplayPage>)
    .toJSON();

  //Assert
  expect(receivedPage).toMatchSnapshot();
  //We now implicitly maintain a 2000 lines long document
  //every additional line break or comment - will break this test
});
```

### 👏 Doing It Right Example: Expectations are visible and focused

```js
it("When visiting TestJavaScript.com home page, a menu is displayed", () => {
  //Arrange

  //Act
  const receivedPage = renderer
    .create(<DisplayPage page="http://www.testjavascript.com"> Test JavaScript </DisplayPage>)
    .toJSON();

  //Assert

  const menu = receivedPage.content.menu;
  expect(menu).toMatchInlineSnapshot(`
<ul>
<li>Home</li>
<li> About </li>
<li> Contact </li>
</ul>
`);
});
```

## ⚪ ️Copy code, but only what's neccessary

✅ **Do:** Include all the necessary details that affect the test result, but nothing more. As an example, consider a test that should factor 100 lines of input JSON - Pasting this in every test is tedious. Extracting it outside to transferFactory.getJSON() will leave the test vague - Without data, it's hard to correlate the test result with the cause ("why is it supposed to return 400 status?"). The classic book x-unit patterns named this pattern'the mystery guest'- Something unseen affected our test results, we don't know what exactly. We can do better by extracting repeatable long parts outside AND mention explictly which specific details matter to the test. Going with the example above, the test can pass parameters that highlight what is important: transferFactory.getJSON({sender: undefined}). In this example, the reader should immediately infer that the empty sender field is the reason why the test should expect a validation error or any other similar adequate outcome.  

❌ **Otherwise:** Copying 500 JSON lines in will leave your tests unmaintainable and unreadable. Moving everything outside will end with vague tests that are hard to understand

✏ **Code Examples**  

### 👎 Anti-Pattern Example: The test failure is unclear because all the cause is external and hides within huge JSON

[![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

```js
test("When no credit, then the transfer is declined", async() => {
      // Arrange
      const transferRequest = testHelpers.factorMoneyTransfer() //get back 200 lines of JSON;
      const transferServiceUnderTest = new TransferService();

      // Act
      const transferResponse = await transferServiceUnderTest.transfer(transferRequest);

      // Assert
      expect(transferResponse.status).toBe(409);// But why do we expect failure: All seems perfectly valid in the test 🤔
    });
```

### 👏 Doing It Right Example: The test highlights what is the cause of the test result

```js
test("When no credit, then the transfer is declined ", async() => {
      // Arrange
      const transferRequest = testHelpers.factorMoneyTransfer({userCredit:100, transferAmount:200}) //obviously there is lack of credit
      const transferServiceUnderTest = new TransferService({disallowOvercharge:true});

      // Act
      const transferResponse = await transferServiceUnderTest.transfer(transferRequest);

      // Assert
      expect(transferResponse.status).toBe(409); // Obviously if the user has no credit it should fail
    });
```

## ⚪ ️ 1.10 Don’t catch errors, expect them

✅ **Do:** When trying to assert that some input triggers an error, it might look right to use try-catch-finally and asserts that the catch clause was entered. The result is an awkward and verbose test case (example below) that hides the simple test intent and the result expectations

A more elegant alternative is the using the one-line dedicated Chai assertion: expect(method).to.throw (or in Jest: expect(method).toThrow()). It’s absolutely mandatory to also ensure the exception contains a property that tells the error type, otherwise given just a generic error the application won’t be able to do much rather than show a disappointing message to the user  

❌ **Otherwise:** It will be challenging to infer from the test reports (e.g. CI reports) what went wrong

✏ **Code Examples**  

### 👎 Anti-pattern Example: A long test case that tries to assert the existence of error with try-catch

[![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

```js
it("When no product name, it throws error 400", async () => {
  let errorWeExceptFor = null;
  try {
    const result = await addNewProduct({});
  } catch (error) {
    expect(error.code).to.equal("InvalidInput");
    errorWeExceptFor = error;
  }
  expect(errorWeExceptFor).not.to.be.null;
  //if this assertion fails, the tests results/reports will only show
  //that some value is null, there won't be a word about a missing Exception
});
```

### 👏 Doing It Right Example: A human-readable expectation that could be understood easily, maybe even by QA or technical PM

```js
it("When no product name, it throws error 400", async () => {
  await expect(addNewProduct({}))
    .to.eventually.throw(AppError)
    .with.property("code", "InvalidInput");
});
```

## ⚪ ️ 1.11 Tag your tests

✅ **Do:** Different tests must run on different scenarios: quick smoke, IO-less, tests should run when a developer saves or commits a file, full end-to-end tests usually run when a new pull request is submitted, etc. This can be achieved by tagging tests with keywords like #cold #api #sanity so you can grep with your testing harness and invoke the desired subset. For example, this is how you would invoke only the sanity test group with Mocha: mocha — grep ‘sanity’  

❌ **Otherwise:** Running all the tests, including tests that perform dozens of DB queries, any time a developer makes a small change can be extremely slow and keeps developers away from running tests

✏ **Code Examples**  

### 👏 Doing It Right Example: Tagging tests as ‘#cold-test’ allows the test runner to execute only fast tests (Cold===quick tests that are doing no IO and can be executed frequently even as the developer is typing)

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
//this test is fast (no DB) and we're tagging it correspondigly
//now the user/CI can run it frequently
describe("Order service", function() {
  describe("Add new order #cold-test #sanity", function() {
    test("Scenario - no currency was supplied. Expectation - Use the default currency #sanity", function() {
      //code logic here
    });
  });
});
```

## ⚪ ️ 1.12 Categorize tests under at least 2 levels

✅ **Do:** Apply some structure to your test suite so an occasional visitor could easily understand the requirements (tests are the best documentation) and the various scenarios that are being tested. A common method for this is by placing at least 2 'describe' blocks above your tests: the 1st is for the name of the unit under test and the 2nd for additional level of categorization like the scenario or custom categories (see code examples and print screen below). Doing so will also greatly improve the test reports: The reader will easily infer the tests categories, delve into the desired section and correlate failing tests. In addition, it will get much easier for a developer to navigate through the code of a suite with many tests. There are multiple alternative structures for test suite that you may consider like [given-when-then](https://github.com/searls/jasmine-given) and [RITE](https://github.com/ericelliott/riteway)

❌ **Otherwise:** When looking at a report with flat and long list of tests, the reader have to skim-read through long texts to conclude the major scenarios and correlate the commonality of failing tests. Consider the following case: When 7/100 tests fail, looking at a flat list will demand reading the failing tests text to see how they relate to each other. However, in a hierarchical report all of them could be under the same flow or category and the reader will quickly infer what or at least where is the root failure cause

✏ **Code Examples**  

### 👏 Doing It Right Example: Structuring suite with the name of unit under test and scenarios will lead to the convenient report that is shown below

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
// Unit under test
describe("Transfer service", () => {
  //Scenario
  describe("When no credit", () => {
    //Expectation
    test("Then the response status should decline", () => {});

    //Expectation
    test("Then it should send email to admin", () => {});
  });
});
```

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/hierarchical-report.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/hierarchical-report.png)

### 👎 Anti-pattern Example: A flat list of tests will make it harder for the reader to identify the user stories and correlate failing tests

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
test("Then the response status should decline", () => {});

test("Then it should send email", () => {});

test("Then there should not be a new transfer record", () => {});
```

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/flat-report.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/flat-report.png)

## ⚪ ️1.13 Other generic good testing hygiene

✅ **Do:** This post is focused on testing advice that is related to, or at least can be exemplified with Node JS. This bullet, however, groups few non-Node related tips that are well-known

Learn and practice [TDD principles](https://www.sm-cloud.com/book-review-test-driven-development-by-example-a-tldr/) — they are extremely valuable for many but don’t get intimidated if they don’t fit your style, you’re not the only one. Consider writing the tests before the code in a [red-green-refactor style](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html), ensure each test checks exactly one thing, when you find a bug — before fixing write a test that will detect this bug in the future, let each test fail at least once before turning green, start a module by writing a quick and simplistic code that satisfies the test - then refactor gradually and take it to a production grade level, avoid any dependency on the environment (paths, OS, etc)  

❌ **Otherwise:** You‘ll miss pearls of wisdom that were collected for decades

## ⚪ ️2.1 Enrich your testing portfolio: Look beyond unit tests and the pyramid

✅ **Do:** The [testing pyramid](https://martinfowler.com/bliki/TestPyramid.html), though 10> years old, is a great and relevant model that suggests three testing types and influences most developers’ testing strategy. At the same time, more than a handful of shiny new testing techniques emerged and are hiding in the shadows of the testing pyramid. Given all the dramatic changes that we’ve seen in the recent 10 years (Microservices, cloud, serverless), is it even possible that one quite-old model will suit _all_ types of applications? shouldn’t the testing world consider welcoming new testing techniques?

Don’t get me wrong, in 2019 the testing pyramid, TDD and unit tests are still a powerful technique and are probably the best match for many applications. Only like any other model, despite its usefulness, [it must be wrong sometimes](https://en.wikipedia.org/wiki/All_models_are_wrong). For example, consider an IoT application that ingests many events into a message-bus like Kafka/RabbitMQ, which then flow into some data-warehouse and are eventually queried by some analytics UI. Should we really spend 50% of our testing budget on writing unit tests for an application that is integration-centric and has almost no logic? As the diversity of application types increase (bots, crypto, Alexa-skills) greater are the chances to find scenarios where the testing pyramid is not the best match.

It’s time to enrich your testing portfolio and become familiar with more testing types (the next bullets suggest few ideas), mind models like the testing pyramid but also match testing types to real-world problems that you’re facing (‘Hey, our API is broken, let’s write consumer-driven contract testing!’), diversify your tests like an investor that build a portfolio based on risk analysis — assess where problems might arise and match some prevention measures to mitigate those potential risks

A word of caution: the TDD argument in the software world takes a typical false-dichotomy face, some preach to use it everywhere, others think it’s the devil. Everyone who speaks in absolutes is wrong :]

❌ **Otherwise:** You’re going to miss some tools with amazing ROI, some like Fuzz, lint, and mutation can provide value in 10 minutes

✏ **Code Examples**  

### 👏 Doing It Right Example: Cindy Sridharan suggests a rich testing portfolio in her amazing post ‘Testing Microservices — the same way’

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-12-rich-testing.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-12-rich-testing.jpeg)

**☺️Example:** [](https://www.youtube.com/watch?v=-2zP494wdUY&feature=youtube)[YouTube: “Beyond Unit Tests: 5 Shiny Node.JS Test Types (2018)” (Yoni Goldberg)](https://www.youtube.com/watch?v=-2zP494wdUY&feature=youtu.be)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-12-Yoni-Goldberg-Testing.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-12-Yoni-Goldberg-Testing.jpeg)

## ⚪ ️2.2 Component testing might be your best affair

✅ **Do:** Each unit test covers a tiny portion of the application and it’s expensive to cover the whole, whereas end-to-end testing easily covers a lot of ground but is flaky and slower, why not apply a balanced approach and write tests that are bigger than unit tests but smaller than end-to-end testing? Component testing is the unsung song of the testing world — they provide the best from both worlds: reasonable performance and a possibility to apply TDD patterns + realistic and great coverage.

Component tests focus on the Microservice ‘unit’, they work against the API, don’t mock anything which belongs to the Microservice itself (e.g. real DB, or at least the in-memory version of that DB) but stub anything that is external like calls to other Microservices. By doing so, we test what we deploy, approach the app from outwards to inwards and gain great confidence in a reasonable amount of time.

[We have a full guide that is solely dedicated to writing component tests in the right way](https://github.com/testjavascript/nodejs-integration-tests-best-practices)

❌ **Otherwise:** You may spend long days on writing unit tests to find out that you got only 20% system coverage

✏ **Code Examples**  

### 👏 Doing It Right Example: Supertest allows approaching Express API in-process (fast and cover many layers)

[![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-13-component-test-yoni-goldberg.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-13-component-test-yoni-goldberg.png)

## ⚪ ️2.3 Ensure new releases don’t break the API using contract tests

✅ **Do:** So your Microservice has multiple clients, and you run multiple versions of the service for compatibility reasons (keeping everyone happy). Then you change some field and ‘boom!’, some important client who relies on this field is angry. This is the Catch-22 of the integration world: It’s very challenging for the server side to consider all the multiple client expectations — On the other hand, the clients can’t perform any testing because the server controls the release dates. There are a spectrum of techniques that can mitigate the contract problem, some are simple, other are more feature-rich and demand a steeper learning curve. In a simple and recommended approach, the API provider publishes npm package with the API typing (e.g. JSDoc, TypeScript). Then the consumers can fetch this library and benefit from codign time intellisense and validation. A fancier approach it to use [PACT](https://docs.pact.io/) which were born to formalize this process with a very disruptive approach — not the server defines the test plan of itself rather the client defines the tests of the… server! PACT can record the client expectation and put in a shared location, “broker”, so the server can pull the expectations and run on every build using PACT library to detect broken contracts — a client expectation that is not met. By doing so, all the server-client API mismatches are caught early during build/CI and might save you a great deal of frustration  

❌ **Otherwise:** The alternatives are exhausting manual testing or deployment fear

✏ **Code Examples**  

### 👏 Doing It Right Example:

[![](https://camo.githubusercontent.com/3720fa51fd963848447150e11b2ee19ed3d3b9adbb6c3943e495a15403749596/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230504143542d626c75652e737667)
](https://camo.githubusercontent.com/3720fa51fd963848447150e11b2ee19ed3d3b9adbb6c3943e495a15403749596/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230504143542d626c75652e737667)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-14-testing-best-practices-contract-flow.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-14-testing-best-practices-contract-flow.png)

## ⚪ ️ 2.4 Test your middlewares in isolation

✅ **Do:** Many avoid Middleware testing because they represent a small portion of the system and require a live Express server. Both reasons are wrong — Middlewares are small but affect all or most of the requests and can be tested easily as pure functions that get {req,res} JS objects. To test a middleware function one should just invoke it and spy ([using Sinon for example](https://www.npmjs.com/package/sinon)) on the interaction with the {req,res} objects to ensure the function performed the right action. The library [node-mock-http](https://www.npmjs.com/package/node-mocks-http) takes it even further and factors the {req,res} objects along with spying on their behavior. For example, it can assert whether the http status that was set on the res object matches the expectation (See example below)  

❌ **Otherwise:** A bug in Express middleware === a bug in all or most requests

✏ **Code Examples**  

### 👏Doing It Right Example: Testing middleware in isolation without issuing network calls and waking-up the entire Express machine

[![](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/6fa089e268604fee5ffc8051441d6391e63506ad1e33162a63c422f44f347e17/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

```js
//the middleware we want to test
const unitUnderTest = require("./middleware");
const httpMocks = require("node-mocks-http");
//Jest syntax, equivelant to describe() & it() in Mocha
test("A request without authentication header, should return http status 403", () => {
  const request = httpMocks.createRequest({
    method: "GET",
    url: "/user/42",
    headers: {
      authentication: ""
    }
  });
  const response = httpMocks.createResponse();
  unitUnderTest(request, response);
  expect(response.statusCode).toBe(403);
});
```

## ⚪ ️2.5 Measure and refactor using static analysis tools

✅ **Do:** Using static analysis tools helps by giving objective ways to improve code quality and keep your code maintainable. You can add static analysis tools to your CI build to abort when it finds code smells. Its main selling points over plain linting are the ability to inspect quality in the context of multiple files (e.g. detect duplications), perform advanced analysis (e.g. code complexity) and follow the history and progress of code issues. Two examples of tools you can use are [SonarQube](https://www.sonarqube.org/) (4,900+ [stars](https://github.com/SonarSource/sonarqube)) and [Code Climate](https://codeclimate.com/) (2,000+ [stars](https://github.com/codeclimate/codeclimate))

Credit: [](https://github.com/TheHollidayInn)[Keith Holliday](https://github.com/TheHollidayInn)

❌ **Otherwise:** With poor code quality, bugs and performance will always be an issue that no shiny new library or state of the art features can fix

✏ **Code Examples**  

### 👏 Doing It Right Example: CodeClimate, a commercial tool that can identify complex methods:

[![](https://camo.githubusercontent.com/a5b32512258db2b18df8c657dee40eaf711d78ea62e12f79cb5806d5ce74aac8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230436f6465253230436c696d6174652d626c75652e737667)
](https://camo.githubusercontent.com/a5b32512258db2b18df8c657dee40eaf711d78ea62e12f79cb5806d5ce74aac8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230436f6465253230436c696d6174652d626c75652e737667)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-16-yoni-goldberg-quality.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-16-yoni-goldberg-quality.png)

## ⚪ ️ 2.6 Check your readiness for Node-related chaos

✅ **Do:** Weirdly, most software testings are about logic & data only, but some of the worst things that happen (and are really hard to mitigate) are infrastructural issues. For example, did you ever test what happens when your process memory is overloaded, or when the server/process dies, or does your monitoring system realizes when the API becomes 50% slower?. To test and mitigate these type of bad things — [Chaos engineering](https://principlesofchaos.org/) was born by Netflix. It aims to provide awareness, frameworks and tools for testing our app resiliency for chaotic issues. For example, one of its famous tools, [the chaos monkey](https://github.com/Netflix/chaosmonkey), randomly kills servers to ensure that our service can still serve users and not relying on a single server (there is also a Kubernetes version, [kube-monkey](https://github.com/asobti/kube-monkey), that kills pods). All these tools work on the hosting/platform level, but what if you wish to test and generate pure Node chaos like check how your Node process copes with uncaught errors, unhandled promise rejection, v8 memory overloaded with the max allowed of 1.7GB or whether your UX remains satisfactory when the event loop gets blocked often? to address this I’ve written, [node-chaos](https://github.com/i0natan/node-chaos-monkey) (alpha) which provides all sort of Node-related chaotic acts  

❌ **Otherwise:** No escape here, Murphy’s law will hit your production without mercy

✏ **Code Examples**  

### 👏 Doing It Right Example: : Node-chaos can generate all sort of Node.js pranks so you can test how resilience is your app to chaos

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-17-yoni-goldberg-chaos-monkey-nodejs.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-17-yoni-goldberg-chaos-monkey-nodejs.png)

## ⚪ ️2.7 Avoid global test fixtures and seeds, add data per-test

✅ **Do:** Going by the golden rule (bullet 0), each test should add and act on its own set of DB rows to prevent coupling and easily reason about the test flow. In reality, this is often violated by testers who seed the DB with data before running the tests (also known as ‘test fixture’) for the sake of performance improvement. While performance is indeed a valid concern — it can be mitigated (see “Component testing” bullet), however, test complexity is a much painful sorrow that should govern other considerations most of the time. Practically, make each test case explicitly add the DB records it needs and act only on those records. If performance becomes a critical concern — a balanced compromise might come in the form of seeding the only suite of tests that are not mutating data (e.g. queries)  

❌ **Otherwise:** Few tests fail, a deployment is aborted, our team is going to spend precious time now, do we have a bug? let’s investigate, oh no — it seems that two tests were mutating the same seed data

✏ **Code Examples**  

### 👎 Anti-Pattern Example: tests are not independent and rely on some global hook to feed global DB data

[![](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)
](https://camo.githubusercontent.com/3707c0dd8e46576830e9cf8769989866d8e610280f144ff17e0fe9ef63b947a5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e672532304d6f6368612d626c75652e737667)

```js
before(async () => {
  //adding sites and admins data to our DB. Where is the data? outside. At some external json or migration framework
  await DB.AddSeedDataFromJson('seed.json');
});
it("When updating site name, get successful confirmation", async () => {
  //I know that site name "portal" exists - I saw it in the seed files
  const siteToUpdate = await SiteService.getSiteByName("Portal");
  const updateNameResult = await SiteService.changeName(siteToUpdate, "newName");
  expect(updateNameResult).to.be(true);
});
it("When querying by site name, get the right site", async () => {
  //I know that site name "portal" exists - I saw it in the seed files
  const siteToCheck = await SiteService.getSiteByName("Portal");
  expect(siteToCheck.name).to.be.equal("Portal"); //Failure! The previous test change the name :[
});
```

### 👏 Doing It Right Example: We can stay within the test, each test acts on its own set of data

```js
it("When updating site name, get successful confirmation", async () => {
  //test is adding a fresh new records and acting on the records only
  const siteUnderTest = await SiteService.addSite({
    name: "siteForUpdateTest"
  });
  const updateNameResult = await SiteService.changeName(siteUnderTest, "newName");
  expect(updateNameResult).to.be(true);
});
```

## ⚪ ️2.8 Choose a clear data clean-up strategy: After-all (recommended) or after-each

✅ **Do:** The timing when the tests clean the database determines the way the tests are being written. The two most viable options are cleaning after all the tests vs cleaning after every single test. Choosing the latter option, cleaning after every single test guarantees clean tables and builds convenient testing perks for the developer. No other records exist when the test starts, one can have certainty which data is being queried and even might be tempted to count rows during assertions. This comes with severe downsides: When running in a multi-process mode, tests are likely to interfere with each other. While process-1 purges tables, at the very moment process-2 queries for data and fail (because the DB was suddenly deleted by process-1). On top of this, It's harder to troubleshoot failing tests - Visiting the DB will show no records.

The second option is to clean up after all the test files have finished (or even daily!). This approach means that the same DB with existing records serves all the tests and processes. To avoid stepping on each other's toes, the tests must add and act on specific records that they have added. Need to check that some record was added? Assume that there are other thousands of records and query for records that were added explicitly. Need to check that a record was deleted? Can't assume an empty table, check that this specific record is not there. This technique brings few powerful gains: It works natively in multi-process mode, when a developer wishes to understand what happened - the data is there and not deleted. It also increases the chance of finding bugs because the DB is full of records and not artificially empty. [See the full comparison table here](https://github.com/testjavascript/nodejs-integration-tests-best-practices/blob/master/graphics/db-clean-options.png).  

❌ **Otherwise:** Without a strategy to separate records or clean - Tests will step on each other toes; Using transactions will work only for relational DB and likely to get complicated once there are inner transactions

✏ **Code Examples**  

### 👏 Cleaning after ALL the tests. Not neccesserily after every run. The more data we have while the tests are running - The more it resembles the production perks

```js
  // After-all clean up (recommended)
// global-teardown.js
module.exports = async () => {
  // ...
  if (Math.ceil(Math.random() * 10) === 10) {
    await new OrderRepository().cleanup();
  }
};
```

## ⚪ ️2.9 Isolate the component from the world using HTTP interceptor

✅ **Do:** Isolate the component under test by intercepting any outgoing HTTP request and providing the desired response so the collaborator HTTP API won't get hit. Nock is a great tool for this mission as it provides a convenient syntax for defining external services behavior. Isolation is a must to prevent noise and slow performance but mostly to simulate various scenarios and responses - A good flight simulator is not about painting clear blue sky rather bringing safe storms and chaos. This is reinforced in a Microservice architecture where the focus should always be on a single component without involving the rest of the world. Though it's possible to simulate external service behavior using test doubles (mocking), it's preferable not to touch the deployed code and act on the network level to keep the tests pure black-box. The downside of isolation is not detecting when the collaborator component changes and not realizing misunderstandings between the two services - Make sure to compensate for this using a few contract or E2E tests  

❌ **Otherwise:** Some services provide a fake version that can be deployed by the caller locally, usually using Docker - This will ease the setup and boost the performance but won't help with simulating various responses; Some services provide'sandbox'environment, so the real service is hit but no costs or side effects are triggered - This will cut down the noise of setting up the 3rd party service but also won't allow simulating scenarios

✏ **Code Examples**  

### 👏 Preventing network calls to externous components allows simulating scnearios and minimizing the noise

````js
// Intercept requests for 3rd party APIs and return a predefined response 
beforeEach(() => {
  nock('http://localhost/user/').get(`/1`).reply(200, {
    id: 1,
    name: 'John',
  });
});```

</details>

## ⚪ ️2.10 Test the response schema, mostly when there are auto-generated fields

:white_check_mark: **Do:** When it is impossible to assert for specific data, check for mandatory field existence and types. Sometimes, the response contains important fields with dynamic data that can't be predicted when writing the test, like dates and incrementing numbers. If the API contract promises that these fields won't be null and hold the right types, it's imperative to test it. Most assertion libraries support checking types. If the response is small, check the return data and type together within the same assertion (see code example). One more option is to verify the entire response against an OpenAPI doc (Swagger). Most test runners have community extensions that validate API responses against their documentation.


<br/>

❌ **Otherwise:** Although the code/API caller relies on some field with dynamic data (e.g., ID, date), it will not come in return and break the contract

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Asserting that fields with dynamic value exist and have the right type

```javascript
  test('When adding a new valid order, Then should get back approval with 200 response', async () => {
  // ...
  //Assert
  expect(receivedAPIResponse).toMatchObject({
    status: 200,
    data: {
      id: expect.any(Number), // Any number satisfies this test
      mode: 'approved',
    },
  });
});
````

## ⚪ ️2.12 Check integrations corner cases and chaos

✅ **Do:** When checking integrations, go beyond the happy and sad paths. Check not only errored responses (e.g., HTTP 500 error) but also network-level anomalies like slow and timed-out responses. This will prove that the code is resilient and can handle various network scenarios like taking the right path after a timeout, has no fragile race conditions, and contains a circuit breaker for retries. Reputable interceptor tools can easily simulate various network behaviors like hectic service that occasionally fail. It can even realize when the default HTTP client timeout value is longer than the simulated response time and throw a timeout exception right away without waiting

❌ **Otherwise:** All your tests pass, it's only the production who will crash or won't report errors correctly when 3rd parties send excpetional responses

✏ **Code Examples**  

### 👏 Ensuring that on network failures, the circuit breaker can save the day

```js
  test('When users service replies with 503 once and retry mechanism is applied, then an order is added successfully', async () => {
  //Arrange
  nock.removeInterceptor(userServiceNock.interceptors[0])
  nock('http://localhost/user/')
    .get('/1')
    .reply(503, undefined, { 'Retry-After': 100 });
  nock('http://localhost/user/')
    .get('/1')
    .reply(200);
  const orderToAdd = {
    userId: 1,
    productId: 2,
    mode: 'approved',
  };

  //Act
  const response = await axiosAPIClient.post('/order', orderToAdd);

  //Assert
  expect(response.status).toBe(200);
});
```

## ⚪ ️2.13 Test the five potential outcomes

✅ **Do:** When planning your tests, consider covering the five typical flow's outputs. When your test is triggering some action (e.g., API call), a reaction is happening, something meaningful occurs and calls for testing. Note that we don't care about how things work. Our focus is on outcomes, things that are noticeable from the outside and might affect the user. These outcomes/reactions can be put in 5 categories:

• Response - The test invokes an action (e.g., via API) and gets a response. It's now concerned with checking the response data correctness, schema, and HTTP status

• A new state - After invoking an action, some **publicly accessible** data is probably modified

• External calls - After invoking an action, the app might call an external component via HTTP or any other transport. For example, a call to send SMS, email or charge a credit card

• Message queues - The outcome of a flow might be a message in a queue

• Observability - Some things must be monitored, like errors or remarkable business events. When a transaction fails, not only we expect the right response but also correct error handling and proper logging/metrics. This information goes directly to a very important user - The ops user (i.e., production SRE/admin)

## ⚪ ️ 3.1 Separate UI from functionality

✅ **Do:** When focusing on testing component logic, UI details become a noise that should be extracted, so your tests can focus on pure data. Practically, extract the desired data from the markup in an abstract way that is not too coupled to the graphic implementation, assert only on pure data (vs HTML/CSS graphic details) and disable animations that slow down. You might get tempted to avoid rendering and test only the back part of the UI (e.g. services, actions, store) but this will result in fictional tests that don't resemble the reality and won't reveal cases where the right data doesn't even arrive in the UI

❌ **Otherwise:** The pure calculated data of your test might be ready in 10ms, but then the whole test will last 500ms (100 tests = 1 min) due to some fancy and irrelevant animation

✏ **Code Examples**  

### 👏 Doing It Right Example: Separating out the UI details

[![](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667)
](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667) [![](https://camo.githubusercontent.com/7ac23d37b16f95d6fb935551bf43c54bb1c5ed33eaa87c45ddbb47efaecda384/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230526561637425323054657374696e672532304c6962726172792d626c75652e737667)
](https://camo.githubusercontent.com/7ac23d37b16f95d6fb935551bf43c54bb1c5ed33eaa87c45ddbb47efaecda384/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230526561637425323054657374696e672532304c6962726172792d626c75652e737667)

```js
test("When users-list is flagged to show only VIP, should display only VIP members", () => {
  // Arrange
  const allUsers = [{ id: 1, name: "Yoni Goldberg", vip: false }, { id: 2, name: "John Doe", vip: true }];

  // Act
  const { getAllByTestId } = render(<UsersList users={allUsers} showOnlyVIP={true} />);

  // Assert - Extract the data from the UI first
  const allRenderedUsers = getAllByTestId("user").map(uiElement => uiElement.textContent);
  const allRealVIPUsers = allUsers.filter(user => user.vip).map(user => user.name);
  expect(allRenderedUsers).toEqual(allRealVIPUsers); //compare data with data, no UI here
});
```

### 👎 Anti-Pattern Example: Assertion mix UI details and data

```js
test("When flagging to show only VIP, should display only VIP members", () => {
  // Arrange
  const allUsers = [{ id: 1, name: "Yoni Goldberg", vip: false }, { id: 2, name: "John Doe", vip: true }];

  // Act
  const { getAllByTestId } = render(<UsersList users={allUsers} showOnlyVIP={true} />);

  // Assert - Mix UI & data in assertion
  expect(getAllByTestId("user")).toEqual('[<li data-test-id="user">John Doe</li>]');
});
```

## ⚪ ️ 3.2 Query HTML elements based on attributes that are unlikely to change

✅ **Do:** Query HTML elements based on attributes that are likely to survive graphic changes unlike CSS selectors and like form labels. If the designated element doesn't have such attributes, create a dedicated test attribute like'test-id-submit-button'. Going this route not only ensures that your functional/logic tests never break because of look & feel changes but also it becomes clear to the entire team that this element and attribute are utilized by tests and shouldn't get removed

❌ **Otherwise:** You want to test the login functionality that spans many components, logic and services, everything is set up perfectly - stubs, spies, Ajax calls are isolated. All seems perfect. Then the test fails because the designer changed the div CSS class from 'thick-border' to 'thin-border'

✏ **Code Examples**  

### 👏 Doing It Right Example: Querying an element using a dedicated attribute for testing

[![](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667)
](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667)

```html
// the markup code (part of React component)
<h3>
  <Badge pill className="fixed_badge" variant="dark">
    <span data-test-id="errorsLabel">{value}</span>
    <!-- note the attribute data-test-id -->
  </Badge>
</h3>
```

```js
// this example is using react-testing-library
test("Whenever no data is passed to metric, show 0 as default", () => {
  // Arrange
  const metricValue = undefined;

  // Act
  const { getByTestId } = render(<dashboardMetric value={undefined} />);

  expect(getByTestId("errorsLabel").text()).toBe("0");
});
```

### 👎 Anti-Pattern Example: Relying on CSS attributes

```html
<!-- the markup code (part of React component) -->
<span id="metric" className="d-flex-column">{value}</span>
<!-- what if the designer changes the classs? -->
```

```js
// this exammple is using enzyme
test("Whenever no data is passed, error metric shows zero", () => {
  // ...

  expect(wrapper.find("[className='d-flex-column']").text()).toBe("0");
});
```

## ⚪ ️ 3.3 Whenever possible, test with a realistic and fully rendered component

✅ **Do:** Whenever reasonably sized, test your component from outside like your users do, fully render the UI, act on it and assert that the rendered UI behaves as expected. Avoid all sort of mocking, partial and shallow rendering - this approach might result in untrapped bugs due to lack of details and harden the maintenance as the tests mess with the internals (see bullet ['Favour blackbox testing'](https://github.com/goldbergyoni/javascript-testing-best-practices#-%EF%B8%8F-14-stick-to-black-box-testing-test-only-public-methods)). If one of the child components is significantly slowing down (e.g. animation) or complicating the setup - consider explicitly replacing it with a fake

With all that said, a word of caution is in order: this technique works for small/medium components that pack a reasonable size of child components. Fully rendering a component with too many children will make it hard to reason about test failures (root cause analysis) and might get too slow. In such cases, write only a few tests against that fat parent component and more tests against its children

❌ **Otherwise:** When poking into a component's internal by invoking its private methods, and checking the inner state - you would have to refactor all tests when refactoring the components implementation. Do you really have a capacity for this level of maintenance?

✏ **Code Examples**  

### 👏 Doing It Right Example: Working realistically with a fully rendered component

[![](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667)
](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667) [![](https://camo.githubusercontent.com/d64bb77cec4924916822b8b895a51378d1090fa0843764ded2880fc47a937116/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230456e7a796d652d626c75652e737667)
](https://camo.githubusercontent.com/d64bb77cec4924916822b8b895a51378d1090fa0843764ded2880fc47a937116/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230456e7a796d652d626c75652e737667)

```js
class Calendar extends React.Component {
  static defaultProps = { showFilters: false };

  render() {
    return (
      <div>
        A filters panel with a button to hide/show filters
        <FiltersPanel showFilter={showFilters} title="Choose Filters" />
      </div>
    );
  }
}

//Examples use React & Enzyme
test("Realistic approach: When clicked to show filters, filters are displayed", () => {
  // Arrange
  const wrapper = mount(<Calendar showFilters={false} />);

  // Act
  wrapper.find("button").simulate("click");

  // Assert
  expect(wrapper.text().includes("Choose Filter"));
  // This is how the user will approach this element: by text
});
```

### 👎 Anti-Pattern Example: Mocking the reality with shallow rendering

```js
test("Shallow/mocked approach: When clicked to show filters, filters are displayed", () => {
  // Arrange
  const wrapper = shallow(<Calendar showFilters={false} title="Choose Filter" />);

  // Act
  wrapper
    .find("filtersPanel")
    .instance()
    .showFilters();
  // Tap into the internals, bypass the UI and invoke a method. White-box approach

  // Assert
  expect(wrapper.find("Filter").props()).toEqual({ title: "Choose Filter" });
  // what if we change the prop name or don't pass anything relevant?
});
```

## ⚪ ️ 3.4 Don't sleep, use frameworks built-in support for async events. Also try to speed things up

✅ **Do:** In many cases, the unit under test completion time is just unknown (e.g. animation suspends element appearance) - in that case, avoid sleeping (e.g. setTimeOut) and prefer more deterministic methods that most platforms provide. Some libraries allows awaiting on operations (e.g. [Cypress cy.request('url')](https://docs.cypress.io/guides/references/best-practices.html#Unnecessary-Waiting)), other provide API for waiting like [@testing-library/dom method wait(expect(element))](https://testing-library.com/docs/guide-disappearance). Sometimes a more elegant way is to stub the slow resource, like API for example, and then once the response moment becomes deterministic the component can be explicitly re-rendered. When depending upon some external component that sleeps, it might turn useful to [hurry-up the clock](https://jestjs.io/docs/en/timer-mocks). Sleeping is a pattern to avoid because it forces your test to be slow or risky (when waiting for a too short period). Whenever sleeping and polling is inevitable and there's no support from the testing framework, some npm libraries like [wait-for-expect](https://www.npmjs.com/package/wait-for-expect) can help with a semi-deterministic solution  

❌ **Otherwise:** When sleeping for a long time, tests will be an order of magnitude slower. When trying to sleep for small numbers, test will fail when the unit under test didn't respond in a timely fashion. So it boils down to a trade-off between flakiness and bad performance

✏ **Code Examples**  

### 👏 Doing It Right Example: E2E API that resolves only when the async operations is done (Cypress)

[![](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)
](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667) [![](https://camo.githubusercontent.com/7ac23d37b16f95d6fb935551bf43c54bb1c5ed33eaa87c45ddbb47efaecda384/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230526561637425323054657374696e672532304c6962726172792d626c75652e737667)
](https://camo.githubusercontent.com/7ac23d37b16f95d6fb935551bf43c54bb1c5ed33eaa87c45ddbb47efaecda384/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230526561637425323054657374696e672532304c6962726172792d626c75652e737667)

```js
// using Cypress
cy.get("#show-products").click(); // navigate
cy.wait("@products"); // wait for route to appear
// this line will get executed only when the route is ready
```

### 👏 Doing It Right Example: Testing library that waits for DOM elements

```js
// @testing-library/dom
test("movie title appears", async () => {
  // element is initially not present...

  // wait for appearance
  await wait(() => {
    expect(getByText("the lion king")).toBeInTheDocument();
  });

  // wait for appearance and return the element
  const movie = await waitForElement(() => getByText("the lion king"));
});
```

### 👎 Anti-Pattern Example: custom sleep code

```js
test("movie title appears", async () => {
  // element is initially not present...

  // custom wait logic (caution: simplistic, no timeout)
  const interval = setInterval(() => {
    const found = getByText("the lion king");
    if (found) {
      clearInterval(interval);
      expect(getByText("the lion king")).toBeInTheDocument();
    }
  }, 100);

  // wait for appearance and return the element
  const movie = await waitForElement(() => getByText("the lion king"));
});
```

## ⚪ ️ 3.5 Watch how the content is served over the network

[![](https://camo.githubusercontent.com/b6c8e22d967ae6c0cae4e3d906592297e9ed0eb036c91bdc48b7222ac5a13ffa/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230476f6f676c652532304c69676874486f7573652d626c75652e737667)
](https://camo.githubusercontent.com/b6c8e22d967ae6c0cae4e3d906592297e9ed0eb036c91bdc48b7222ac5a13ffa/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230476f6f676c652532304c69676874486f7573652d626c75652e737667)

✅ **Do:** Apply some active monitor that ensures the page load under real network is optimized - this includes any UX concern like slow page load or un-minified bundle. The inspection tools market is no short: basic tools like [pingdom](https://www.pingdom.com/), AWS CloudWatch, [gcp StackDriver](https://cloud.google.com/monitoring/uptime-checks/) can be easily configured to watch whether the server is alive and response under a reasonable SLA. This only scratches the surface of what might get wrong, hence it's preferable to opt for tools that specialize in frontend (e.g. [lighthouse](https://developers.google.com/web/tools/lighthouse/), [pagespeed](https://developers.google.com/speed/pagespeed/insights/)) and perform richer analysis. The focus should be on symptoms, metrics that directly affect the UX, like page load time, [meaningful paint](https://scotch.io/courses/10-web-performance-audit-tips-for-your-next-billion-users-in-2018/fmp-first-meaningful-paint), [time until the page gets interactive (TTI)](https://calibreapp.com/blog/time-to-interactive/). On top of that, one may also watch for technical causes like ensuring the content is compressed, time to the first byte, optimize images, ensuring reasonable DOM size, SSL and many others. It's advisable to have these rich monitors both during development, as part of the CI and most important - 24x7 over the production's servers/CDN

❌ **Otherwise:** It must be disappointing to realize that after such great care for crafting a UI, 100% functional tests passing and sophisticated bundling - the UX is horrible and slow due to CDN misconfiguration

✏ **Code Examples**

### 👏 Doing It Right Example: Lighthouse page load inspection report

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/lighthouse2.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/lighthouse2.png)

## ⚪ ️ 3.6 Stub flaky and slow resources like backend APIs

✅ **Do:** When coding your mainstream tests (not E2E tests), avoid involving any resource that is beyond your responsibility and control like backend API and use stubs instead (i.e. test double). Practically, instead of real network calls to APIs, use some test double library (like [Sinon](https://sinonjs.org/), [Test doubles](https://www.npmjs.com/package/testdouble), etc) for stubbing the API response. The main benefit is preventing flakiness - testing or staging APIs by definition are not highly stable and from time to time will fail your tests although YOUR component behaves just fine (production env was not meant for testing and it usually throttles requests). Doing this will allow simulating various API behavior that should drive your component behavior as when no data was found or the case when API throws an error. Last but not least, network calls will greatly slow down the tests

❌ **Otherwise:** The average test runs no longer than few ms, a typical API call last 100ms>, this makes each test ~20x slower

✏ **Code Examples**  

### 👏 Doing It Right Example: Stubbing or intercepting API calls

[![](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667)
](https://camo.githubusercontent.com/a11ca15f4f098d12f63c2b5af5cc46e26a48a6113fe20db47ceccc060f58fd27/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e6725323052656163742d626c75652e737667) [![](https://camo.githubusercontent.com/7ac23d37b16f95d6fb935551bf43c54bb1c5ed33eaa87c45ddbb47efaecda384/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230526561637425323054657374696e672532304c6962726172792d626c75652e737667)
](https://camo.githubusercontent.com/7ac23d37b16f95d6fb935551bf43c54bb1c5ed33eaa87c45ddbb47efaecda384/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541372532304578616d706c652532307573696e67253230526561637425323054657374696e672532304c6962726172792d626c75652e737667)

```js
// unit under test
export default function ProductsList() {
  const [products, setProducts] = useState(false);

  const fetchProducts = async () => {
    const products = await axios.get("api/products");
    setProducts(products);
  };

  useEffect(() => {
    fetchProducts();
  }, []);

  return products ? <div>{products}</div> : <div data-test-id="no-products-message">No products</div>;
}

// test
test("When no products exist, show the appropriate message", () => {
  // Arrange
  nock("api")
    .get(`/products`)
    .reply(404);

  // Act
  const { getByTestId } = render(<ProductsList />);

  // Assert
  expect(getByTestId("no-products-message")).toBeTruthy();
});
```

## ⚪ ️ 3.7 Have very few end-to-end tests that spans the whole system

✅ **Do:** Although E2E (end-to-end) usually means UI-only testing with a real browser (See [bullet 3.6](https://github.com/goldbergyoni/javascript-testing-best-practices#-%EF%B8%8F-36-stub-flaky-and-slow-resources-like-backend-apis)), for other they mean tests that stretch the entire system including the real backend. The latter type of tests is highly valuable as they cover integration bugs between frontend and backend that might happen due to a wrong understanding of the exchange schema. They are also an efficient method to discover backend-to-backend integration issues (e.g. Microservice A sends the wrong message to Microservice B) and even to detect deployment failures - there are no backend frameworks for E2E testing that are as friendly and mature as UI frameworks like [Cypress](https://www.cypress.io/) and [Puppeteer](https://github.com/GoogleChrome/puppeteer). The downside of such tests is the high cost of configuring an environment with so many components, and mostly their brittleness - given 50 microservices, even if one fails then the entire E2E just failed. For that reason, we should use this technique sparingly and probably have 1-10 of those and no more. That said, even a small number of E2E tests are likely to catch the type of issues they are targeted for - deployment & integration faults. It's advisable to run those over a production-like staging environment

❌ **Otherwise:** UI might invest much in testing its functionality only to realizes very late that the backend returned payload (the data schema the UI has to work with) is very different than expected

## ⚪ ️ 3.8 Speed-up E2E tests by reusing login credentials

✅ **Do:** In E2E tests that involve a real backend and rely on a valid user token for API calls, it doesn't payoff to isolate the test to a level where a user is created and logged-in in every request. Instead, login only once before the tests execution start (i.e. before-all hook), save the token in some local storage and reuse it across requests. This seem to violate one of the core testing principle - keep the test autonomous without resources coupling. While this is a valid worry, in E2E tests performance is a key concern and creating 1-3 API requests before starting each individual tests might lead to horrible execution time. Reusing credentials doesn't mean the tests have to act on the same user records - if relying on user records (e.g. test user payments history) than make sure to generate those records as part of the test and avoid sharing their existence with other tests. Also remember that the backend can be faked - if your tests are focused on the frontend it might be better to isolate it and stub the backend API (see [bullet 3.6](https://github.com/goldbergyoni/javascript-testing-best-practices#-%EF%B8%8F-36-stub-flaky-and-slow-resources-like-backend-apis)).

❌ **Otherwise:** Given 200 test cases and assuming login=100ms = 20 seconds only for logging-in again and again

✏ **Code Examples**  

### 👏 Doing It Right Example: Logging-in before-all and not before-each

[![](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)
](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)

```js
let authenticationToken;

// happens before ALL tests run
before(() => {
  cy.request('POST', 'http://localhost:3000/login', {
    username: Cypress.env('username'),
    password: Cypress.env('password'),
  })
  .its('body')
  .then((responseFromLogin) => {
    authenticationToken = responseFromLogin.token;
  })
})

// happens before EACH test
beforeEach(setUser => () {
  cy.visit('/home', {
    onBeforeLoad (win) {
      win.localStorage.setItem('token', JSON.stringify(authenticationToken))
    },
  })
})
```

## ⚪ ️ 3.9 Have one E2E smoke test that just travels across the site map

✅ **Do:** For production monitoring and development-time sanity check, run a single E2E test that visits all/most of the site pages and ensures no one breaks. This type of test brings a great return on investment as it's very easy to write and maintain, but it can detect any kind of failure including functional, network and deployment issues. Other styles of smoke and sanity checking are not as reliable and exhaustive - some ops teams just ping the home page (production) or developers who run many integration tests which don't discover packaging and browser issues. Goes without saying that the smoke test doesn't replace functional tests rather just aim to serve as a quick smoke detector

❌ **Otherwise:** Everything might seem perfect, all tests pass, production health-check is also positive but the Payment component had some packaging issue and only the /Payment route is not rendering

✏ **Code Examples**  

### 👏 Doing It Right Example: Smoke travelling across all pages

[![](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)
](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)

## ⚪ ️ 3.10 Expose the tests as a live collaborative document

✅ **Do:** Besides increasing app reliability, tests bring another attractive opportunity to the table - serve as live app documentation. Since tests inherently speak at a less-technical and product/UX language, using the right tools they can serve as a communication artifact that greatly aligns all the peers - developers and their customers. For example, some frameworks allow expressing the flow and expectations (i.e. tests plan) using a human-readable language so any stakeholder, including product managers, can read, approve and collaborate on the tests which just became the live requirements document. This technique is also being referred to as 'acceptance test' as it allows the customer to define his acceptance criteria in plain language. This is [BDD (behavior-driven testing)](https://en.wikipedia.org/wiki/Behavior-driven_development) at its purest form. One of the popular frameworks that enable this is [Cucumber which has a JavaScript flavor](https://github.com/cucumber/cucumber-js), see example below. Another similar yet different opportunity, [StoryBook](https://storybook.js.org/), allows exposing UI components as a graphic catalog where one can walk through the various states of each component (e.g. render a grid w/o filters, render that grid with multiple rows or with none, etc), see how it looks like, and how to trigger that state - this can appeal also to product folks but mostly serves as live doc for developers who consume those components.

❌ **Otherwise:** After investing top resources on testing, it's just a pity not to leverage this investment and win great value

✏ **Code Examples**  

### 👏 Doing It Right Example: Describing tests in human-language using cucumber-js

[![](https://camo.githubusercontent.com/bfef4eeb511fd9488079367a5f7b7c0ff10ca8b39c51e430376cfe9e3d780a81/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437563756d6265722d626c75652e737667)
](https://camo.githubusercontent.com/bfef4eeb511fd9488079367a5f7b7c0ff10ca8b39c51e430376cfe9e3d780a81/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437563756d6265722d626c75652e737667)

```js
// this is how one can describe tests using cucumber: plain language that allows anyone to understand and collaborate

Feature: Twitter new tweet

  I want to tweet something in Twitter

  @focus
  Scenario: Tweeting from the home page
    Given I open Twitter home
    Given I click on "New tweet" button
    Given I type "Hello followers!" in the textbox
    Given I click on "Submit" button
    Then I see message "Tweet saved"
```

### 👏 Doing It Right Example: Visualizing our components, their various states and inputs using Storybook

[![](https://camo.githubusercontent.com/e58105b40280768773aab5c88f86938cd5ebb888ce6fcb25cc4c2d8f43ed6b10/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e6725323053746f7279426f6f6b2d626c75652e737667)
](https://camo.githubusercontent.com/e58105b40280768773aab5c88f86938cd5ebb888ce6fcb25cc4c2d8f43ed6b10/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e6725323053746f7279426f6f6b2d626c75652e737667)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/story-book.jpg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/story-book.jpg)

## ⚪ ️ 3.11 Detect visual issues with automated tools

✅ **Do:** Setup automated tools to capture UI screenshots when changes are presented and detect visual issues like content overlapping or breaking. This ensures that not only the right data is prepared but also the user can conveniently see it. This technique is not widely adopted, our testing mindset leans toward functional tests but it's the visuals what the user experience and with so many device types it's very easy to overlook some nasty UI bug. Some free tools can provide the basics - generate and save screenshots for the inspection of human eyes. While this approach might be sufficient for small apps, it's flawed as any other manual testing that demands human labor anytime something changes. On the other hand, it's quite challenging to detect UI issues automatically due to the lack of clear definition - this is where the field of 'Visual Regression' chime in and solve this puzzle by comparing old UI with the latest changes and detect differences. Some OSS/free tools can provide some of this functionality (e.g. [wraith](https://github.com/BBC-News/wraith), PhantomCSS but might charge significant setup time. The commercial line of tools (e.g. [Applitools](https://applitools.com/), [Percy.io](https://percy.io/)) takes is a step further by smoothing the installation and packing advanced features like management UI, alerting, smart capturing by eliminating 'visual noise' (e.g. ads, animations) and even root cause analysis of the DOM/CSS changes that led to the issue

❌ **Otherwise:** How good is a content page that display great content (100% tests passed), loads instantly but half of the content area is hidden?

✏ **Code Examples**  

### 👎 Anti-Pattern Example: A typical visual regression - right content that is served badly

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/amazon-visual-regression.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/amazon-visual-regression.jpeg)

### 👏 Doing It Right Example: Configuring wraith to capture and compare UI snapshots

[![](https://camo.githubusercontent.com/c84dc8dbd72873030c9f8bcfeba203c094b0475bc7e3445450d6b50441480f2d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532305772616974682d626c75652e737667)
](https://camo.githubusercontent.com/c84dc8dbd72873030c9f8bcfeba203c094b0475bc7e3445450d6b50441480f2d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532305772616974682d626c75652e737667)

    ​# Add as many domains as necessary. Key will act as a label​

    domains:
      english: "[http://www.mysite.com](http://www.mysite.com/)"​

    ​# Type screen widths below, here are a couple of examples​

    screen_widths:

      - 600​
      - 768​
      - 1024​
      - 1280​

    ​# Type page URL paths below, here are a couple of examples​
    paths:
      about:
        path: /about
        selector: '.about'​
      subscribe:
          selector: '.subscribe'​
        path: /subscribe 

### 👏 Doing It Right Example: Using Applitools to get snapshot comparison and other advanced features

[![](https://camo.githubusercontent.com/6d4aa085b2e57d6d2aef4a53966fa35730b5a83b59fbb87cd016d0a7babd68f1/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532304170706c69546f6f6c732d626c75652e737667)
](https://camo.githubusercontent.com/6d4aa085b2e57d6d2aef4a53966fa35730b5a83b59fbb87cd016d0a7babd68f1/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532304170706c69546f6f6c732d626c75652e737667) [![](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)
](https://camo.githubusercontent.com/5abf3eb8aca2eac0d390eb88b95412cdf703d68089ea103a173aeb9984b7ff98/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230437970726573732d626c75652e737667)

```js
import * as todoPage from "../page-objects/todo-page";

describe("visual validation", () => {
  before(() => todoPage.navigate());
  beforeEach(() => cy.eyesOpen({ appName: "TAU TodoMVC" }));
  afterEach(() => cy.eyesClose());

  it("should look good", () => {
    cy.eyesCheckWindow("empty todo list");
    todoPage.addTodo("Clean room");
    todoPage.addTodo("Learn javascript");
    cy.eyesCheckWindow("two todos");
    todoPage.toggleTodo(0);
    cy.eyesCheckWindow("mark as completed");
  });
});
```

## ⚪ ️ 4.1 Get enough coverage for being confident, ~80% seems to be the lucky number

✅ **Do:** The purpose of testing is to get enough confidence for moving fast, obviously the more code is tested the more confident the team can be. Coverage is a measure of how many code lines (and branches, statements, etc) are being reached by the tests. So how much is enough? 10–30% is obviously too low to get any sense about the build correctness, on the other side 100% is very expensive and might shift your focus from the critical paths to the exotic corners of the code. The long answer is that it depends on many factors like the type of application — if you’re building the next generation of Airbus A380 than 100% is a must, for a cartoon pictures website 50% might be too much. Although most of the testing enthusiasts claim that the right coverage threshold is contextual, most of them also mention the number 80% as a thumb of a rule ([Fowler: “in the upper 80s or 90s”](https://martinfowler.com/bliki/TestCoverage.html)) that presumably should satisfy most of the applications.

Implementation tips: You may want to configure your continuous integration (CI) to have a coverage threshold ([Jest link](https://jestjs.io/docs/en/configuration.html#collectcoverage-boolean)) and stop a build that doesn’t stand to this standard (it’s also possible to configure threshold per component, see code example below). On top of this, consider detecting build coverage decrease (when a newly committed code has less coverage) — this will push developers raising or at least preserving the amount of tested code. All that said, coverage is only one measure, a quantitative based one, that is not enough to tell the robustness of your testing. And it can also be fooled as illustrated in the next bullets

❌ **Otherwise:** Confidence and numbers go hand in hand, without really knowing that you tested most of the system — there will also be some fear and fear will slow you down

✏ **Code Examples**  

### 👏 Example: A typical coverage report

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-18-yoni-goldberg-code-coverage.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-18-yoni-goldberg-code-coverage.png)

### 👏 Doing It Right Example: Setting up coverage per component (using Jest)

[![](https://camo.githubusercontent.com/e371adf51ef873c66982acb531bc21809b69073474927a36cfa94d798b63cf44/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532304a6573742d626c75652e737667)
](https://camo.githubusercontent.com/e371adf51ef873c66982acb531bc21809b69073474927a36cfa94d798b63cf44/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e672532304a6573742d626c75652e737667)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-18-code-coverage2.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-18-code-coverage2.jpeg)

## ⚪ ️ 4.2 Inspect coverage reports to detect untested areas and other oddities

✅ **Do:** Some issues sneak just under the radar and are really hard to find using traditional tools. These are not really bugs but more of surprising application behavior that might have a severe impact. For example, often some code areas are never or rarely being invoked — you thought that the ‘PricingCalculator’ class is always setting the product price but it turns out it is actually never invoked although we have 10000 products in DB and many sales… Code coverage reports help you realize whether the application behaves the way you believe it does. Other than that, it can also highlight which types of code is not tested — being informed that 80% of the code is tested doesn’t tell whether the critical parts are covered. Generating reports is easy — just run your app in production or during testing with coverage tracking and then see colorful reports that highlight how frequent each code area is invoked. If you take your time to glimpse into this data — you might find some gotchas  

❌ **Otherwise:** If you don’t know which parts of your code are left un-tested, you don’t know where the issues might come from

✏ **Code Examples**  

### 👎 Anti-Pattern Example: What’s wrong with this coverage report?

Based on a real-world scenario where we tracked our application usage in QA and find out interesting login patterns (Hint: the amount of login failures is non-proportional, something is clearly wrong. Finally it turned out that some frontend bug keeps hitting the backend login API)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-19-coverage-yoni-goldberg-nodejs-consultant.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-19-coverage-yoni-goldberg-nodejs-consultant.png)

## ⚪ ️ 4.3 Measure logical coverage using mutation testing

✅ **Do:** The Traditional Coverage metric often lies: It may show you 100% code coverage, but none of your functions, even not one, return the right response. How come? it simply measures over which lines of code the test visited, but it doesn’t check if the tests actually tested anything — asserted for the right response. Like someone who’s traveling for business and showing his passport stamps — this doesn’t prove any work done, only that he visited few airports and hotels.

Mutation-based testing is here to help by measuring the amount of code that was actually TESTED not just VISITED. [Stryker](https://stryker-mutator.io/) is a JavaScript library for mutation testing and the implementation is really neat:

(1) it intentionally changes the code and “plants bugs”. For example the code newOrder.price===0 becomes newOrder.price!=0. This “bugs” are called mutations

(2) it runs the tests, if all succeed then we have a problem — the tests didn’t serve their purpose of discovering bugs, the mutations are so-called survived. If the tests failed, then great, the mutations were killed.

Knowing that all or most of the mutations were killed gives much higher confidence than traditional coverage and the setup time is similar  

❌ **Otherwise:** You’ll be fooled to believe that 85% coverage means your test will detect bugs in 85% of your code

✏ **Code Examples**  

### 👎 Anti-Pattern Example: 100% coverage, 0% testing

[![](https://camo.githubusercontent.com/32188f42ebfb0dc4f34f6fc1911ee6c4489784a0335e96f80037b642540dc738/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230537472796b65722d626c75652e737667)
](https://camo.githubusercontent.com/32188f42ebfb0dc4f34f6fc1911ee6c4489784a0335e96f80037b642540dc738/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539342541382532304578616d706c652532307573696e67253230537472796b65722d626c75652e737667)

```js
function addNewOrder(newOrder) {
  logger.log(`Adding new order ${newOrder}`);
  DB.save(newOrder);
  Mailer.sendMail(newOrder.assignee, `A new order was places ${newOrder}`);

  return { approved: true };
}

it("Test addNewOrder, don't use such test names", () => {
  addNewOrder({ assignee: "John@mailer.com", price: 120 });
}); //Triggers 100% code coverage, but it doesn't check anything
```

### 👏 Doing It Right Example: Stryker reports, a tool for mutation testing, detects and counts the amount of code that is not tested (Mutations)

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-20-yoni-goldberg-mutation-testing.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-20-yoni-goldberg-mutation-testing.jpeg)

## ⚪ ️4.4 Preventing test code issues with Test linters

✅ **Do:** A set of ESLint plugins were built specifically for inspecting the tests code patterns and discover issues. For example, [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) will warn when a test is written at the global level (not a son of a describe() statement) or when tests are [skipped](https://mochajs.org/#inclusive-tests) which might lead to a false belief that all tests are passing. Similarly, [eslint-plugin-jest](https://github.com/jest-community/eslint-plugin-jest) can, for example, warn when a test has no assertions at all (not checking anything)

❌ **Otherwise:** Seeing 90% code coverage and 100% green tests will make your face wear a big smile only until you realize that many tests aren’t asserting for anything and many test suites were just skipped. Hopefully, you didn’t deploy anything based on this false observation

✏ **Code Examples**  

### 👎 Anti-Pattern Example: A test case full of errors, luckily all are caught by Linters

```js
describe("Too short description", () => {
  const userToken = userService.getDefaultToken() // *error:no-setup-in-describe, use hooks (sparingly) instead
  it("Some description", () => {});//* error: valid-test-description. Must include the word "Should" + at least 5 words
});

it.skip("Test name", () => {// *error:no-skipped-tests, error:error:no-global-tests. Put tests only under describe or suite
  expect("somevalue"); // error:no-assert
});

it("Test name", () => {*//error:no-identical-title. Assign unique titles to tests
});
```

## ⚪ ️ 5.1 Enrich your linters and abort builds that have linting issues

✅ **Do:** Linters are a free lunch, with 5 min setup you get for free an auto-pilot guarding your code and catching significant issue as you type. Gone are the days where linting was about cosmetics (no semi-colons!). Nowadays, Linters can catch severe issues like errors that are not thrown correctly and losing information. On top of your basic set of rules (like [ESLint standard](https://www.npmjs.com/package/eslint-plugin-standard) or [Airbnb style](https://www.npmjs.com/package/eslint-config-airbnb)), consider including some specializing Linters like [eslint-plugin-chai-expect](https://www.npmjs.com/package/eslint-plugin-chai-expect) that can discover tests without assertions, [eslint-plugin-promise](https://www.npmjs.com/package/eslint-plugin-promise?activeTab=readme) can discover promises with no resolve (your code will never continue), [eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security?activeTab=readme) which can discover eager regex expressions that might get used for DOS attacks, and [eslint-plugin-you-dont-need-lodash-underscore](https://www.npmjs.com/package/eslint-plugin-you-dont-need-lodash-underscore) is capable of alarming when the code uses utility library methods that are part of the V8 core methods like Lodash.\_map(…)  

❌ **Otherwise:** Consider a rainy day where your production keeps crashing but the logs don’t display the error stack trace. What happened? Your code mistakenly threw a non-error object and the stack trace was lost, a good reason for banging your head against a brick wall. A 5 min linter setup could detect this TYPO and save your day

✏ **Code Examples**  

### 👎 Anti-Pattern Example: The wrong Error object is thrown mistakenly, no stack-trace will appear for this error. Luckily, ESLint catches the next production bug

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-21-yoni-goldberg-eslint.jpeg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-21-yoni-goldberg-eslint.jpeg)

## ⚪ ️ 5.2 Shorten the feedback loop with local developer-CI

✅ **Do:** Using a CI with shiny quality inspections like testing, linting, vulnerabilities check, etc? Help developers run this pipeline also locally to solicit instant feedback and shorten the [feedback loop](https://www.gocd.org/2016/03/15/are-you-ready-for-continuous-delivery-part-2-feedback-loops/). Why? an efficient testing process constitutes many and iterative loops: (1) try-outs -> (2) feedback -> (3) refactor. The faster the feedback is, the more improvement iterations a developer can perform per-module and perfect the results. On the flip, when the feedback is late to come fewer improvement iterations could be packed into a single day, the team might already move forward to another topic/task/module and might not be up for refining that module.

Practically, some CI vendors (Example: [CircleCI local CLI](https://circleci.com/docs/2.0/local-cli/)) allow running the pipeline locally. Some commercial tools like [wallaby provide highly-valuable & testing insights](https://wallabyjs.com/) as a developer prototype (no affiliation). Alternatively, you may just add npm script to package.json that runs all the quality commands (e.g. test, lint, vulnerabilities) — use tools like [concurrently](https://www.npmjs.com/package/concurrently) for parallelization and non-zero exit code if one of the tools failed. Now the developer should just invoke one command — e.g. ‘npm run quality’ — to get instant feedback. Consider also aborting a commit if the quality check failed using a githook ([husky can help](https://github.com/typicode/husky))  

❌ **Otherwise:** When the quality results arrive the day after the code, testing doesn’t become a fluent part of development rather an after the fact formal artifact

✏ **Code Examples**  

### 👏 Doing It Right Example: npm scripts that perform code quality inspection, all are run in parallel on demand or when a developer is trying to push new code

```js
"scripts": {
    "inspect:sanity-testing": "mocha **/**--test.js --grep \"sanity\"",
    "inspect:lint": "eslint .",
    "inspect:vulnerabilities": "npm audit",
    "inspect:license": "license-checker --failOn GPLv2",
    "inspect:complexity": "plato .",

    "inspect:all": "concurrently -c \"bgBlue.bold,bgMagenta.bold,yellow\" \"npm:inspect:quick-testing\" \"npm:inspect:lint\" \"npm:inspect:vulnerabilities\" \"npm:inspect:license\""
  },
  "husky": {
    "hooks": {
      "precommit": "npm run inspect:all",
      "prepush": "npm run inspect:all"
    }
}
```

## ⚪ ️5.3 Perform e2e testing over a true production-mirror

✅ **Do:** End to end (e2e) testing are the main challenge of every CI pipeline — creating an identical ephemeral production mirror on the fly with all the related cloud services can be tedious and expensive. Finding the best compromise is your game: [Docker-compose](https://serverless.com/) allows crafting isolated dockerized environment with identical containers using a single plain text file but the backing technology (e.g. networking, deployment model) is different from real-world productions. You may combine it with [‘AWS Local’](https://github.com/localstack/localstack) to work with a stub of the real AWS services. If you went [serverless](https://serverless.com/) multiple frameworks like serverless and [AWS SAM](https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html) allows the local invocation of FaaS code.

The huge Kubernetes ecosystem is yet to formalize a standard convenient tool for local and CI-mirroring though many new tools are launched frequently. One approach is running a ‘minimized-Kubernetes’ using tools like [Minikube](https://kubernetes.io/docs/setup/minikube/) and [MicroK8s](https://microk8s.io/) which resemble the real thing only come with less overhead. Another approach is testing over a remote ‘real-Kubernetes’, some CI providers (e.g. [Codefresh](https://codefresh.io/)) has native integration with Kubernetes environment and make it easy to run the CI pipeline over the real thing, others allow custom scripting against a remote Kubernetes.  

❌ **Otherwise:** Using different technologies for production and testing demands maintaining two deployment models and keeps the developers and the ops team separated

✏ **Code Examples**  

### 👏 Example: a CI pipeline that generates Kubernetes cluster on the fly [(](https://container-solutions.com/dynamic-environments-kubernetes/)[Credit: Dynamic-environments Kubernetes](https://container-solutions.com/dynamic-environments-kubernetes/))

deploy:  
stage: deploy  
image: registry.gitlab.com/gitlab-examples/kubernetes-deploy  
script:  
- ./configureCluster.sh $KUBE_CA_PEM_FILE $KUBE_URL $KUBE_TOKEN  
- kubectl create ns $NAMESPACE  
- kubectl create secret -n $NAMESPACE docker-registry gitlab-registry --docker-server="$CI_REGISTRY"--docker-username="$CI_REGISTRY_USER"--docker-password="$CI_REGISTRY_PASSWORD"--docker-email="$GITLAB_USER_EMAIL"  
- mkdir .generated  
- echo "$CI_BUILD_REF_NAME-$CI_BUILD_REF"  
- sed -e "s/TAG/$CI_BUILD_REF_NAME-$CI_BUILD_REF/g"templates/deals.yaml | tee".generated/deals.yaml"  
- kubectl apply --namespace $NAMESPACE -f .generated/deals.yaml  
- kubectl apply --namespace $NAMESPACE -f templates/my-sock-shop.yaml  
environment:  
name: test-for-ci

## ⚪ ️5.4 Parallelize test execution

✅ **Do:** When done right, testing is your 24/7 friend providing almost instant feedback. In practice, executing 500 CPU-bounded unit test on a single thread can take too long. Luckily, modern test runners and CI platforms (like [Jest](https://github.com/facebook/jest), [AVA](https://github.com/avajs/ava) and [Mocha extensions](https://github.com/yandex/mocha-parallel-tests)) can parallelize the test into multiple processes and achieve significant improvement in feedback time. Some CI vendors do also parallelize tests across containers (!) which shortens the feedback loop even further. Whether locally over multiple processes, or over some cloud CLI using multiple machines — parallelizing demand keeping the tests autonomous as each might run on different processes

❌ **Otherwise:** Getting test results 1 hour long after pushing new code, as you already code the next features, is a great recipe for making testing less relevant

✏ **Code Examples**  

### 👏 Doing It Right Example: Mocha parallel & Jest easily outrun the traditional Mocha thanks to testing parallelization ([Credit: JavaScript Test-Runners Benchmark](https://medium.com/dailyjs/javascript-test-runners-benchmark-3a78d4117b4))

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-24-yonigoldberg-jest-parallel.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-24-yonigoldberg-jest-parallel.png)

## ⚪ ️5.5 Stay away from legal issues using license and plagiarism check

✅ **Do:** Licensing and plagiarism issues are probably not your main concern right now, but why not tick this box as well in 10 minutes? A bunch of npm packages like [license check](https://www.npmjs.com/package/license-checker) and [plagiarism check](https://www.npmjs.com/package/plagiarism-checker) (commercial with free plan) can be easily baked into your CI pipeline and inspect for sorrows like dependencies with restrictive licenses or code that was copy-pasted from Stack Overflow and apparently violates some copyrights

❌ **Otherwise:** Unintentionally, developers might use packages with inappropriate licenses or copy paste commercial code and run into legal issues

✏ **Code Examples**  

### 👏 Doing It Right Example:

```js
//install license-checker in your CI environment or also locally
npm install -g license-checker

//ask it to scan all licenses and fail with exit code other than 0 if it found unauthorized license. The CI system should catch this failure and stop the build
license-checker --summary --failOn BSD
```

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-25-nodejs-licsense.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-25-nodejs-licsense.png)

## ⚪ ️5.6 Constantly inspect for vulnerable dependencies

✅ **Do:** Even the most reputable dependencies such as Express have known vulnerabilities. This can get easily tamed using community tools such as [npm audit](https://docs.npmjs.com/getting-started/running-a-security-audit), or commercial tools like [snyk](https://snyk.io/) (offer also a free community version). Both can be invoked from your CI on every build

❌ **Otherwise:** Keeping your code clean from vulnerabilities without dedicated tools will require to constantly follow online publications about new threats. Quite tedious

✏ **Code Examples**  

### 👏 Example: NPM Audit result

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-26-npm-audit-snyk.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-26-npm-audit-snyk.png)

## ⚪ ️5.7 Automate dependency updates

✅ **Do:** Yarn and npm latest introduction of package-lock.json introduced a serious challenge (the road to hell is paved with good intentions) — by default now, packages are no longer getting updates. Even a team running many fresh deployments with ‘npm install’ & ‘npm update’ won’t get any new updates. This leads to subpar dependent packages versions at best or to vulnerable code at worst. Teams now rely on developers goodwill and memory to manually update the package.json or use tools [like ncu](https://www.npmjs.com/package/npm-check-updates) manually. A more reliable way could be to automate the process of getting the most reliable dependency versions, though there are no silver bullet solutions yet there are two possible automation roads:

(1) CI can fail builds that have obsolete dependencies — using tools like [‘npm outdated’](https://docs.npmjs.com/cli/outdated) or ‘npm-check-updates (ncu)’ . Doing so will enforce developers to update dependencies.

(2) Use commercial tools that scan the code and automatically send pull requests with updated dependencies. One interesting question remaining is what should be the dependency update policy — updating on every patch generates too many overhead, updating right when a major is released might point to an unstable version (many packages found vulnerable on the very first days after being released, [see the](https://nodesource.com/blog/a-high-level-post-mortem-of-the-eslint-scope-security-incident/) eslint-scope incident).

An efficient update policy may allow some ‘vesting period’ — let the code lag behind the @latest for some time and versions before considering the local copy as obsolete (e.g. local version is 1.3.1 and repository version is 1.3.8)  

❌ **Otherwise:** Your production will run packages that have been explicitly tagged by their author as risky

✏ **Code Examples**  

### 👏 Example: [ncu](https://www.npmjs.com/package/npm-check-updates) can be used manually or within a CI pipeline to detect to which extent the code lag behind the latest versions

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/bp-27-yoni-goldberg-npm.png)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/bp-27-yoni-goldberg-npm.png)

## ⚪ ️ 5.8 Other, non-Node related, CI tips

✅ **Do:** This post is focused on testing advice that is related to, or at least can be exemplified with Node JS. This bullet, however, groups few non-Node related tips that are well-known

1.  Use a declarative syntax. This is the only option for most vendors but older versions of Jenkins allows using code or UI
2.  Opt for a vendor that has native Docker support
3.  Fail early, run your fastest tests first. Create a ‘Smoke testing’ step/milestone that groups multiple fast inspections (e.g. linting, unit tests) and provide snappy feedback to the code committer
4.  Make it easy to skim-through all build artifacts including test reports, coverage reports, mutation reports, logs, etc
5.  Create multiple pipelines/jobs for each event, reuse steps between them. For example, configure a job for feature branch commits and a different one for master PR. Let each reuse logic using shared steps (most vendors provide some mechanism for code reuse)
6.  Never embed secrets in a job declaration, grab them from a secret store or from the job’s configuration
7.  Explicitly bump version in a release build or at least ensure the developer did so
8.  Build only once and perform all the inspections over the single build artifact (e.g. Docker image)
9.  Test in an ephemeral environment that doesn’t drift state between builds. Caching node_modules might be the only exception

❌ **Otherwise:** You‘ll miss years of wisdom

## ⚪ ️ 5.9 Build matrix: Run the same CI steps using multiple Node versions

✅ **Do:** Quality checking is about serendipity, the more ground you cover the luckier you get in detecting issues early. When developing reusable packages or running a multi-customer production with various configuration and Node versions, the CI must run the pipeline of tests over all the permutations of configurations. For example, assuming we use MySQL for some customers and Postgres for others — some CI vendors support a feature called ‘Matrix’ which allow running the suit of testing against all permutations of MySQL, Postgres and multiple Node version like 8, 9 and 10. This is done using configuration only without any additional effort (assuming you have testing or any other quality checks). Other CIs who doesn’t support Matrix might have extensions or tweaks to allow that  

❌ **Otherwise:** So after doing all that hard work of writing testing are we going to let bugs sneak in only because of configuration issues?

✏ **Code Examples**  

### 👏 Example: Using Travis (CI vendor) build definition to run the same test over multiple Node versions

language: node_js  
node_js:  

-   "7"  
-   "6"  
-   "5"  
-   "4"  
    install:  
-   npm install  
    script:  
-   npm run test

## Yoni Goldberg

[![](https://github.com/goldbergyoni/javascript-testing-best-practices/raw/master/assets/yoni-goldberg.jpg)
](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/assets/yoni-goldberg.jpg)

**Role:** Writer

**About:** I'm an independent consultant who works with Fortune 500 companies and garage startups on polishing their JS & Node.js applications. More than any other topic I'm fascinated by and aims to master the art of testing. I'm also the author of [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

**📗 Online Course:** Liked this guide and wish to take your testing skills to the extreme? Consider visiting my comprehensive course [Testing Node.js & JavaScript From A To Z](https://www.testjavascript.com/)

**Follow:**

-   [🐦 Twitter](https://twitter.com/goldbergyoni/)
-   [📞 Contact](https://testjavascript.com/contact-2/)
-   [✉️ Newsletter](https://testjavascript.com/newsletter//)

* * *

## [Bruno Scheufler](https://github.com/BrunoScheufler)

**Role:** Tech reviewer and advisor

Took care to revise, improve, lint and polish all the texts

**About:** full-stack web engineer, Node.js & GraphQL enthusiast

* * *

## [Ido Richter](https://github.com/idori)

**Role:** Concept, design and great advice

**About:** A savvy frontend developer, CSS expert and emojis freak

## [Kyle Martin](https://github.com/js-kyle)

**Role:** Helps keep this project running, and reviews security related practices

**About:** Loves working on Node.js projects and web application security.

## Contributors ✨

Thanks goes to these wonderful people who have contributed to this repository!

| \[![](https://avatars3.githubusercontent.com/u/1326248?v=4?s=100)

**Scott Davis**]([http://geospatialscott.blogspot.com/](http://geospatialscott.blogspot.com/))  
[🖋](#content-stdavis "Content") | \[![](https://avatars2.githubusercontent.com/u/5978436?v=4?s=100)

**Adrien REDON**]([https://github.com/AdrienRedon](https://github.com/AdrienRedon))  
[🖋](#content-AdrienRedon "Content") | \[![](https://avatars0.githubusercontent.com/u/173663?v=4?s=100)

**Stefano Magni**]([https://twitter.com/NoriSte](https://twitter.com/NoriSte))  
[🖋](#content-NoriSte "Content") | \[![](https://avatars2.githubusercontent.com/u/47742486?v=4?s=100)

**Yeoh Joer**]([https://www.joer.im/](https://www.joer.im/))  
[🖋](#content-yjoer "Content") | \[![](https://avatars0.githubusercontent.com/u/2177742?v=4?s=100)

**Jhonny Moreira**]([http://jhonnymoreira.dev/](http://jhonnymoreira.dev/))  
[🖋](#content-jhonnymoreira "Content") | \[![](https://avatars2.githubusercontent.com/u/8846678?v=4?s=100)

**Ian Germann**]([https://github.com/Germanika](https://github.com/Germanika))  
[🖋](#content-Germanika "Content") | \[![](https://avatars3.githubusercontent.com/u/19984935?v=4?s=100)

**Hafez**]([https://github.com/AbdelrahmanHafez](https://github.com/AbdelrahmanHafez))  
[🖋](#content-AbdelrahmanHafez "Content") \|
| \[![](https://avatars1.githubusercontent.com/u/11021586?v=4?s=100)

**Ruxandra Fediuc**]([http://www.ruxandrafediuc.com/](http://www.ruxandrafediuc.com/))  
[🖋](#content-ruxandrafed "Content") | \[![](https://avatars0.githubusercontent.com/u/9951291?v=4?s=100)

**Jack**]([https://github.com/jacklee814](https://github.com/jacklee814))  
[🖋](#content-jacklee814 "Content") | \[![](https://avatars0.githubusercontent.com/u/231727?v=4?s=100)

**Peter Carrero**]([https://www.petercarrero.com/](https://www.petercarrero.com/))  
[🖋](#content-aloyr "Content") | \[![](https://avatars3.githubusercontent.com/u/369338?v=4?s=100)

**Huhgawz**]([https://github.com/huhgawz](https://github.com/huhgawz))  
[🖋](#content-huhgawz "Content") | \[![](https://avatars1.githubusercontent.com/u/7099302?v=4?s=100)

**Haakon Borch**]([https://github.com/haakonmb](https://github.com/haakonmb))  
[🖋](#content-haakonmb "Content") | \[![](https://avatars3.githubusercontent.com/u/5395811?v=4?s=100)

**Jaime Mendoza**]([https://jaimemendoza.com/](https://jaimemendoza.com/))  
[🖋](#content-jaimemendozadev "Content") | \[![](https://avatars0.githubusercontent.com/u/840612?v=4?s=100)

**Cameron Dunford**]([https://github.com/camerondunford](https://github.com/camerondunford))  
[🖋](#content-camerondunford "Content") \|
| \[![](https://avatars1.githubusercontent.com/u/15719847?v=4?s=100)

**John Gee**]([https://github.com/shadowspawn](https://github.com/shadowspawn))  
[🖋](#content-shadowspawn "Content") | \[![](https://avatars0.githubusercontent.com/u/3273544?v=4?s=100)

**Aurelijus Rožėnas**]([https://github.com/aurelijusrozenas](https://github.com/aurelijusrozenas))  
[🖋](#content-aurelijusrozenas "Content") | \[![](https://avatars2.githubusercontent.com/u/42848750?v=4?s=100)

**Aaron**]([http://aaronshivers.com/](http://aaronshivers.com/))  
[🖋](#content-aaronshivers "Content") | \[![](https://avatars1.githubusercontent.com/u/8683577?v=4?s=100)

**Tom Nagle**]([https://tomdoes.tech/](https://tomdoes.tech/))  
[🖋](#content-tomanagle "Content") | \[![](https://avatars0.githubusercontent.com/u/7723729?v=4?s=100)

**Yves yao**]([https://github.com/yvesyao](https://github.com/yvesyao))  
[🖋](#content-yvesyao "Content") | \[![](https://avatars1.githubusercontent.com/u/34487074?v=4?s=100)

**Userbit**]([https://github.com/Userbit](https://github.com/Userbit))  
[🖋](#content-Userbit "Content") | \[![](https://avatars0.githubusercontent.com/u/1631477?v=4?s=100)

**Glaucia Lemos**]([https://glaucialemos.netlify.com/](https://glaucialemos.netlify.com/))  
[🚧](#maintenance-glaucia86 "Maintenance") \|
| \[![](https://avatars2.githubusercontent.com/u/7419215?v=4?s=100)

**koooge**]([https://twitter.com/koooge](https://twitter.com/koooge))  
[🖋](#content-koooge "Content") | \[![](https://avatars0.githubusercontent.com/u/18367606?v=4?s=100)

**Michal**]([https://twitter.com/michalbiesiada](https://twitter.com/michalbiesiada))  
[🖋](#content-mbiesiad "Content") | \[![](https://avatars0.githubusercontent.com/u/611846?v=4?s=100)

**roywalker**]([http://roywalker.me/](http://roywalker.me/))  
[🖋](#content-roywalker "Content") | \[![](https://avatars3.githubusercontent.com/u/23185799?v=4?s=100)

**dangen**]([https://dangen-effy.github.io/](https://dangen-effy.github.io/))  
[🖋](#content-dangen-effy "Content") | \[![](https://avatars1.githubusercontent.com/u/60202305?v=4?s=100)

**biesiadamich**]([https://dev.to/mbiesiad](https://dev.to/mbiesiad))  
[🖋](#content-biesiadamich "Content") | \[![](https://avatars3.githubusercontent.com/u/127009?v=4?s=100)

**Yanlin Jiang**]([https://tarojsx.github.io/](https://tarojsx.github.io/))  
[🖋](#content-cncolder "Content") | \[![](https://avatars2.githubusercontent.com/u/2077168?v=4?s=100)

**sanguino**]([https://github.com/sanguino](https://github.com/sanguino))  
[🖋](#content-sanguino "Content") \|
| \[![](https://avatars0.githubusercontent.com/u/3721240?v=4?s=100)

**Morgan**]([https://github.com/MorganGeek](https://github.com/MorganGeek))  
[🖋](#content-MorganGeek "Content") | \[![](https://avatars0.githubusercontent.com/u/8350985?v=4?s=100)

**Lukas Bischof**]([https://luk4s.dev/](https://luk4s.dev/))  
[⚠️](https://github.com/goldbergyoni/javascript-testing-best-practices/commits?author=lukasbischof "Tests") [🖋](#content-lukasbischof "Content") | \[![](https://avatars2.githubusercontent.com/u/1837650?v=4?s=100)

**JuanMa Ruiz**]([https://juanmaruiz.surge.sh/](https://juanmaruiz.surge.sh/))  
[🖋](#content-JuanMaRuiz "Content") | \[![](https://avatars3.githubusercontent.com/u/22268900?v=4?s=100)

**Luís Ângelo Rodrigues Jr.**]([https://luisangelorjr.com.br/](https://luisangelorjr.com.br/))  
[🖋](#content-luisangelorjr "Content") | \[![](https://avatars0.githubusercontent.com/u/12046620?v=4?s=100)

**José Fernández**]([https://jfernandezpe.wordpress.com/](https://jfernandezpe.wordpress.com/))  
[🖋](#content-jfernandezpe "Content") | \[![](https://avatars3.githubusercontent.com/u/56408597?v=4?s=100)

**Alejandro Gutierrez Barcenilla**]([http://www.linkedin.com/in/AlejandroGutierrezB](http://www.linkedin.com/in/AlejandroGutierrezB))  
[🖋](#content-AlejandroGutierrezB "Content") | \[![](https://avatars1.githubusercontent.com/u/30088000?v=4?s=100)

**Jason**]([https://github.com/jasonandmonte](https://github.com/jasonandmonte))  
[🖋](#content-jasonandmonte "Content") \|
| \[![](https://avatars.githubusercontent.com/u/11263232?v=4?s=100)

**Otavio Araujo**]([https://github.com/otavionetoca](https://github.com/otavionetoca))  
[⚠️](https://github.com/goldbergyoni/javascript-testing-best-practices/commits?author=otavionetoca "Tests") [🖋](#content-otavionetoca "Content") | \[![](https://avatars.githubusercontent.com/u/5027939?v=4?s=100)

**Alex Ivanov**]([https://contributor.pw/](https://contributor.pw/))  
[🖋](#content-contributorpw "Content") | \[![](https://avatars.githubusercontent.com/u/20400822?v=4?s=100)

**Yiqiao Xu**]([https://github.com/YeeJone](https://github.com/YeeJone))  
[🖋](#content-YeeJone "Content") | \[![](https://avatars.githubusercontent.com/u/31545456?v=4?s=100)

**YuBin, Hsu**]([https://github.com/yubinTW](https://github.com/yubinTW))  
[🌍](#translation-yubinTW "Translation") [💻](https://github.com/goldbergyoni/javascript-testing-best-practices/commits?author=yubinTW "Code") \| 
 [https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme.md](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme.md)
