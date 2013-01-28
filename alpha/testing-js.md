Why you should test your JavaScript code
========================================

THIS IS STILL UNDER DEVELOPMENT.

Because JavaScript
------------------

JavaScript has no compiler support, which many of us are used too from
Java and C#. Additionally our IDEs have less support for working with
JavaScript because it's such a dynamic language. For these reasons we
can be far less confident in our JavaScript code than our Java code.

It's also important to remember that JavaScript runs in varied
environments, often from IE7 to the latest nightly build of Chromium.
Having a test for that strange IE bug will ensure that we don't
introduce the same erroneous bit of code time and time again.

Enabling larger refactorings
----------------------------

What we experienced on our team was that good tests enabled us to do
larger refactorings and still be confident that we didn't break anything
--- and without going through the entire app in the browser after we
changed some code.

Tools are getting better
------------------------

The last few years the JavaScript testing tools have improved
significantly. There are still many problems, but tools such as
[Jasmine](http://pivotal.github.com/jasmine/),
[QUnit](http://docs.jquery.com/QUnit) and
[Sinon.js](http://sinonjs.org/) are without a doubt good enough —
they give more value than they are a pain to work with.

Code quality
------------

The last project I worked at had, by far, the best JavaScript I have
ever worked with. It was also the first long project I have worked on
that really put an effort into testing the JavaScript code. Throughout
the next sections we will see some reasons as to why JavaScript tests
might help with improving your JavaScript code.

### Less complexity

This looks like JavaScript I find in my old projects:

```javascript
function populateDropdownWithPersons(select) {
  $.getJSON('http://localhost:1337/persons.json', function(data) {
    var options = [];

    $.each(data, function(key, val) {
      options.push('<option value="' + val.id + '">' + val.name +
          ' (' + (val.project || 'ukjent') + ')</option>');
    });

    select.html(options.join(''));
  });
}
```

So, what's the problem with this little bit of code?

**It does too much!**

These 9 lines of code makes an Ajax requests, parses the response,
builds some HTML, and manipulates the DOM. In these 9 lines we have
coupled together fetching data from the server with the DOM. This little
bit of code is not easily reusable and it's a pain to test.

When driving JavaScript code through tests I (almost) never end up
writing code like this.

### Reusability

When your code is more focused, it's easier to reuse. Let's try to split
out one aspect of the previous example into a function that does one
thing. For example, what if we moved the `select` and `option` stuff
into an `optionsMapper`?

```javascript
var Utils = {};
Utils.optionsMapper = function (array, opts) {
  var options = [];

  for(var i = 0, length = array.length; i < length; i++) {
    var obj = array[i];
    options.push('<option value="' + obj[opts.value] + '">' + 
        obj[opts.text] + '</option>');
  }

  return options;
}

// Using the optionsMapper
// ... and let's just say we have a persons array

var options = Utils.optionsMapper(persons, {
  value: 'id',
  text: 'name'
});
```

This is highly reusable and very easy to test. And it does *one* thing.

In the beginning this feels like the wrong thing to do. To solve all the
four aspects of the first example, we will end up with far more than 9
lines of code. But the important aspect is thinking 3k lines ahead. At
some point you will have so many reusable bits of code that developing
will feel so much easier and so much faster.

### Maintainability

Additionally, when creating simpler more focused functions they are far
easier to maintain. It's easier to work with a function that does one
thing than one that does four. And, remember, you will most likely use
far more time on maintaining your JavaScript than on writing new code.

So what does it mean to write maintainable JavaScript code? There are at
least three aspects that spring to my mind; it should be understandable,
intuitive and extendable. One essential aspect of this is having loose
coupling between components. What I've experienced is that driving your
JavaScript architecture through tests help with all of these.

Documentation
-------------

Good tests document why the code does what it does. When I'm no longer
on the project and someone else need to understand why this one function
does what it does, they can look at the tests and most likely get the
answer. At least they won't break the intended functionality as long as
it is properly tested.

Less dependent on one person
----------------------------

A lot of the JavaScript I've seen is so complex it's really just the
person who wrote it that can work efficiently with it. And that's not
ok when we start writing more and more JavaScript every day.

Because you would if it was Java
--------------------------------

So just apply the same reasons you apply when convincing yourself that
testing your Java code is the right thing to do.

You should treat JavaScript as a real language.

It's not some magic potion
--------------------------

Writing good tests is not simple. It's actually quite hard. And writing
tests will not magically make your code fantastic. But it will help. At
least it did for me.
