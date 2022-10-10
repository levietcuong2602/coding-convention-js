## I. Javascript Coding convention

### 1.1. How to Name Variables

It can take some time to find a good name but it will save you and your team even more time in the future. And I am sure most readers have faced the situation where you visit your code only a few months later and have a hard time understanding what you did before.

### Using meaningful and pronounceable variable names

Do not use comments to explain why a variable is used. If a name requires a comment, then you should take your time to rename that variable instead of writing a comment.

A name should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

**Bad**

```JS
var d; // elapsed time in days
```

It is a common misconception that you should hide your mess with comments. Do not use letters like x, y, z, a, b, c as variable names unless there is a good reason (loop variable is an exception to this).

**Good**

```JS
var elapsedTimeInDays;
var daysSinceCreation;
var daysSinceModification;
```

### Using Pronounceable Names

If you can't pronounce a name, you can't discuss it without sounding silly.

**Bad**

```JS
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good**

```JS
const currentDate = moment().format("YYYY/MM/DD");
```

### Using Searchable Names

Avoid using magic numbers in your code. Opt for searchable, named constants. Do not use single-letter names for constants since they can appear in many places and therefore they are not easily searchable.

**Bad**

```JS
if (student.classes.length < 7) {
   // Do something
}
```

**Good**

```JS
if (student.classes.length < MAX_CLASSES_PER_STUDENT) {
    // Do something
}
```

This is much better because **MAX_CLASSES_PER_STUDENT** can used in many places in code. If we need to change it to 6 in the future, we can just change the constant.

### Don't add unneeded context

If your class/object name tells you something, don't repeat that in your variable name.

**Bad**

```JS
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

```JS
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

```JS
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good**

```JS
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

```JS
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

```JS
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

```JS
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good**

```JS
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

### Encapsulate Conditionals in Functions

Refactoring the condition and putting it into a named function is a good way to make your conditionals more readable.

**Bad**

```JS
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good**

```JS
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

```JS
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**Good**

```JS
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

```JS
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good**

```JS
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

```JS
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

```JS
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

```JS
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

```JS
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

```JS

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

```JS
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

```JS
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

```JS
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

```JS
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

```JS
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

```JS
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good**

```JS
doStuff();
```

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

**Bad**

```JS
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

```JS
function combine(a, b) {
  return a + b;
}
```

## II. Best practice REST API

In Web Development, REST APIs play an important role in ensuring smooth communication between the client and the server.

Communication between the client (frontend) and the server (backend) isn't usually super direct. So we use an interface called an Application Programming Interface (or API) to act as an intermediary between the client and the server.

Because API plays a crucial role in this client–server communication, we should always design APIs with best practices in mind. This helps the developers maintaining them, and those consuming them as well, not run into issues while performing those duties.

### Use JSON as the Format for Sending and Receiving Data

In the past, accepting and responding to API requests were done mostly in XML and even HTML. But these days, JSON (JavaScript Object Notation) has largely become the de-facto format for sending and receiving API data.

To ensure the client interprets JSON data correctly, you should set the Content-Type type in the response header to application/json while making the request.

### Use Nouns Instead of Verbs in Endpoints

When you're designing a REST API, you should not use verbs in the endpoint paths. The endpoints should use nouns, signifying what each of them does.

This is because HTTP methods such as GET, POST, PUT, PATCH, and DELETE are already in verb form for performing basic CRUD (Create, Read, Update, Delete) operations.

GET, POST, PUT, PATCH, and DELETE are the commonest HTTP verbs. There are also others such as COPY, PURGE, LINK, UNLINK, and so on.

**Bad**

```JS
https://mysite.com/getPosts
https://mysite.com/createPost
```

**Good**

```JS
GET   https://mysite.com/posts
POST  https://mysite.com/posts
```

Instead, it should be something like this: https://mysite.com/posts

In short, you should let the HTTP verbs handle what the endpoints do. So GET would retrieve data, POST will create data, PUT will update data, and DELETE will get rid of the data.

### Name Collections with Plural Nouns

You can think of the data of your API as a collection of different resources from your consumers.

**Bad**

```JS
https://mysite.com/post/123
```

**Good**

```JS
https://mysite.com/posts/123
```

If you have an endpoint like https://mysite.com/post/123 but `post` it doesn’t tell the user that there could be some other posts in the collection. This is why your collections should use plural nouns.

So, instead of https://mysite.com/post/123, it should be https://mysite.com/posts/123.

### Use Status Codes in Error Handling

You should always use regular HTTP status codes in responses to requests made to your API. This will help your users to know what is going on – whether the request is successful, or if it fails, or something else.

```JS
200 OK – Trả về thành công cho những phương thức GET, PUT, PATCH hoặc DELETE.
201 Created – Trả về khi một Resouce vừa được tạo thành công.
204 No Content – Trả về khi Resource xoá thành công.
304 Not Modified – Client có thể sử dụng dữ liệu cache, resource server không đổi gì.
400 Bad Request – Request không hợp lệ
401 Unauthorized – Request cần có xác thực.
403 Forbidden – bị từ chối không cho phép.
404 Not Found – Không tìm thấy resource từ URI
405 Method Not Allowed – Phương thức không cho phép với user hiện tại.
410 Gone – Resource không còn tồn tại, Version cũ đã không còn hỗ trợ.
415 Unsupported Media Type – Không hỗ trợ kiểu Resource này.
422 Unprocessable Entity – Dữ liệu không được xác thực
429 Too Many Requests – Request bị từ chối do bị giới hạn
```

### Use Nesting on Endpoints to Show Relationships

Oftentimes, different endpoints can be interlinked, so you should nest them so it's easier to understand them.

For example, in the case of a multi-user blogging platform, different posts could be written by different authors, so an endpoint such as:

```JS
https://mysite.com/posts/author
```

would make a valid nesting in this case.

In the same vein, the posts might have their individual comments, so to retrieve the comments, an endpoint like:

```JS
https://mysite.com/posts/postId/comments
```

would make sense.

You should avoid nesting that is more than 3 levels deep as this can make the API less elegant and readable.

### Use Filtering, Sorting, and Pagination to Retrieve the Data Requested

Sometimes, an API's database can get incredibly large. If this happens, retrieving data from such a database could be very slow.

Filtering, sorting, and pagination are all actions that can be performed on the collection of a REST API. This lets it only retrieve, sort, and arrange the necessary data into pages so the server doesn’t get too occupied with requests.

An example of a filtered endpoint is the one below:

```JS
https://mysite.com/posts?tags=javascript
```

This endpoint will fetch any post that has a tag of JavaScript.

### Use SSL for Security

SSL stands for secure socket layer. It is crucial for security in REST API design. This will secure your API and make it less vulnerable to malicious attacks.

The clear difference between the URL of a REST API that runs over SSL and the one which does not is the “s” in HTTP:

```JS
 https://mysite.com/posts runs on SSL.
```

```JS
http://mysite.com/posts does not run on SSL.
```

### Be Clear with Versioning

REST APIs should have different versions, so you don’t force clients (users) to migrate to new versions. This might even break the application if you're not careful.

One of the commonest versioning systems in web development is `semantic versioning`.

An example of semantic versioning is 1.0.0, 2.1.2, and 3.3.4

Many RESTful APIs from tech giants and individuals usually comes like this:

```JS
https://mysite.com/v1/ for version 1
```

```JS
https://mysite.com/v2 for version 2
```

### Provide Accurate API Documentation

When you make a REST API, you need to help clients (consumers) learn and figure out how to use it correctly. The best way to do this is by providing good documentation for the API.
The documentation should contain:

- Relevant endpoints of the API
- Example requests of the endpoints
- Implementation in several programming languages
- Messages listed for different errors with their status codes.

One of the most common tools you can use for API documentation is Swagger. And you can also use Postman, one of the most common API testing tools in software development, to document your APIs.
