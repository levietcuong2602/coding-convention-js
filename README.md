## Javascript Coding convention

### How to Name Variables

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
