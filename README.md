## I. Javascript Coding convention

### 1.1. How to Name Variables

It can take some time to find a good name but it will save you and your team even more time in the future. And I am sure most readers have faced the situation where you visit your code only a few months later and have a hard time understanding what you did before.

### Using meaningful and pronounceable variable names

Do not use comments to explain why a variable is used. If a name requires a comment, then you should take your time to rename that variable instead of writing a comment.

A name should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

**Bad**

```
var d; // elapsed time in days
```

It is a common misconception that you should hide your mess with comments. Do not use letters like x, y, z, a, b, c as variable names unless there is a good reason (loop variable is an exception to this).

**Good**

```
var elapsedTimeInDays;
var daysSinceCreation;
var daysSinceModification;
```

### Using Pronounceable Names

If you can't pronounce a name, you can't discuss it without sounding silly.

**Bad**

```
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good**

```
const currentDate = moment().format("YYYY/MM/DD");
```

### Using Searchable Names

Avoid using magic numbers in your code. Opt for searchable, named constants. Do not use single-letter names for constants since they can appear in many places and therefore they are not easily searchable.

**Bad**

```
if (student.classes.length < 7) {
   // Do something
}
```

**Good**

```
if (student.classes.length < MAX_CLASSES_PER_STUDENT) {
    // Do something
}
```

This is much better because **MAX_CLASSES_PER_STUDENT** can used in many places in code. If we need to change it to 6 in the future, we can just change the constant.

### Don't add unneeded context

If your class/object name tells you something, don't repeat that in your variable name.

**Bad**

```
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**Good**

```
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

### Use default parameters instead of short circuiting or conditionals (Sử dụng những tham số mặc định thay vì kiểm tra các điều kiện lòng vòng)

Default parameters are often cleaner than circuiting. Be aware that if you use them, your function will only provider default values for `undefinded`. Other "falsy" value such `''`, `""`, `false`, `null`, `0` and `NaN`, will not be replaced by a default value.

**Bad**

```
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good**

```
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

### 1.2. How to Write Functions

### Keep them Small

Functions should be small, really small. They should rarely be 20 lines long. The longer a function gets, it is more likely it is to do multiple things and have side effects.

### Make Sure They Just Do One Thing

Your functions should do only one thing. If you follow this rule, it is guaranteed that they will be small. The only thing that function does should be stated in its name.

Sometimes it is hard to look at the function and see if it is doing multiple things or not. One good way to check is to try to extract another function with a different name. If you can find it, that means it should be a different function.

Once you get the hang of it, your code will look much more mature, and it will be more easily refactorable, understandable, and testable for sure.

**Bad**

```
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good**

```
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

### Function names should say what they do

**Bad**

```
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good**

```
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

### Encapsulate Conditionals in Functions

Refactoring the condition and putting it into a named function is a good way to make your conditionals more readable.

**Bad**

```
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good**

```
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

### Fewer Arguments

Functions should have two or fewer arguments, the fewer the better. Avoid three or more arguments where possible.

Arguments make it harder to read and understand the function. They are even harder from a testing point of view, since they create the need to write test cases for every combination of arguments.

**Bad**

```
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**Good**

```
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

### Do not use Flag Arguments

Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

**Bad**

```
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good**

```
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

### Do Not Have Side Effects

Side effects are unintended consequences of your code. They may be changing the passed parameters, in case of passing by reference, or maybe changing a global variable.

The key point is, they promised to do another thing and you need to read the code carefully to notice the side-effect. They can result in some nasty bugs.

**Bad**

```
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good**

```
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

### Don't Repeat Yourself

Do your absolute best to avoid duplicate code. Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.

**Bad**

```
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good**

```
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

### 1.3. How to formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is DO NOT ARGUE over formatting. There are tons of tools to automate this as like: `eslint`, `prettier`... Use one! It's a waste of time and money for engineers to argue over formatting.

### Use consistent capitalization

JavaScript is untyped, so capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just be consistent.

Constant variables should be capitalized

**Bad**

```

const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good**

```
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee. We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad**

```
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**Good**

```
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

### 1.4. How to write comments

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. Good code mostly documents itself.

**Bad**

```
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Good**

```
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad**

```
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good**

```
doStuff();
```

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

**Bad**

```
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Good**

```
function combine(a, b) {
  return a + b;
}
```

## II. Best practice REST API

In Web Development, REST APIs play an important role in ensuring smooth communication between the client and the server.
