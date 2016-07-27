# Embedding JavaScript into SharePoint

You can use namespaces to avoid conflicts between your JavaScript customizations and standard SharePoint JavaScript or JavaScript customizations deployed by other developers. 

The [OfficeDev/PnP](https://github.com/OfficeDev/PnP/) samples and solutions often include JavaScript code. In order to make the techniques easy to understand, these samples are usually simple and do not use namespaces when embedding JavaScript code into SharePoint. It is important to ensure that you follow the simple steps outlined in this article when you incorporate PnP samples into your solutions.

## Why using namespaces is important
<a name="sectionSection0"> </a>

JavaScript is a loosely typed language. If you define a variable or function, and a variable or function with the same name already exists in the current context, the new value or implementation will replace the existing one.
As a result, when you embed JavaScript code into SharePoint, it is easy to override standard SharePoint JavaScript code or customizations deployed by other developers.
This can create conflicts that might be hard to identify and debug.

To avoid this, we recommend that you use custom namespaces for your JavaScript code.

## How to use namespaces
<a name="sectionSection1"> </a>

The following example shows a simple pattern used to organize JavaScript code in namespaces and classes.

```JavaScript
var MySolution = MySolution || {};

MySolution.MyClass1 = (function () {
    // private members
    var privateVar1 = 1;
    var privateVar2 = 2;
    
    function privateFunction1(){
      return "";
    }
    
    return {
        // public interface
        myFunction1: function() {
          return privateVar1;
        }
        myFunction2: function(){
          return privateVar2;
        }
    };
})();
```

Functions defined in public interface can be invoked as:

```JavaScript
MySolution.MyClass1.myFunction1();

MySolution.MyClass1.myFunction2();
```

Because all your code uses the custom *MySolution* namespace, you can avoid any naming conflicts.

## Minimal Download Strategy

When MDS is enabled it will clear out all global Namespaces on navigation.

To retain your Namespace,

instead of declaring it as:

```JavaScript
var MySolution = MySolution || {};
```

declare your Namespace as:

```JavaScript
    Type.registerNamespace('MySolution');
```

The ``Type`` Namespace is SharePoint specific and not available in the New OD4B Document Library View

If you want your Namespace declared for any environment, use:

```JavaScript
if (window.hasOwnProperty('Type')) {
    Type.registerNamespace('MySolution');
} else {
    window.MySolution = window.MySolution || {};
}
```
