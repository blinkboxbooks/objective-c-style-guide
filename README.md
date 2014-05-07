#Objective-C Style Guide

---


The Objective-C Style Guide used by blinkbox books

##Whitespaces
---
* Spaces or tabs?
* Braces should always open at the same line as the keyword `if,else,switch, for,while, method, functions` and close with the new line



```
if (someBooleanValue){
 // do stuff
}
else if(someOtherBooleanValue){
 // do something
}
else{
 // do other 
}
```

* methods should have single space separating type of method (`+` or `-` sign) from the returned value type (void in the example below). Another space should be put after returned value type. Spaces should also separate each parameter of the method and asterisk from the class name. There should _NOT_ be a space between beginning of the line and method type indicator nor between name of the last parameter and the opening brace

```
- (void) someMethodName:(BBBParameterClass *)parameterName 
       anotherParameter:(NSFunnyClass *)anotherNiceParameter{
 // method body
}
```

##Documentation
---
* All public methods should be documented using AppleDoc style 
* Comments can be both single line (`//`) or block style (`/**/`)
* Use `#pragma mark - text` to organise code inside your implementation  file

##Organisation
---
* Instance variables usage is prohibited outside custom setters/getters and init/dealloc methods
* Always use opening and closing braces for control structures (if,else,switch, for,while) even if there is only single statement inside it (we don't want another goto fail; )
* Usage of string literals in the code is discouraged, as well as numeric values. We should use const variables  whenever applicable.
* Subclasses of base Cocoa classes should have proper suffixes for easier finding, examples:
	1.  `UIViewController` subclasses should be called `BBB..........ViewController`,
	2.  `UIView` subclasses should be called `BBB...View`, 
	3.  `UITableViewCell` subclasses should be called `BBB...[TableView]Cell`



##Localisation
---
* Never use string NSString literals for user-visible strings, always use NSLocalizedString() macro
* Use reverse DNS schema for user visible strings, this greatly improve ease of possible future localisation

``` 
label.text = NSLocalizedString(@"library-screen.button-title.main-menu",nil);
```

* When presenting date values to user, always use NSDateFormatter, NSDateComponent API
* For displaying content with flexible plurality or gender, we should use Localized Property List File (Localizable.stringsdict) format. Unicode Refence, OSX 10.9 reference 

##Errors and exceptions
---
* Use `NSExceptions` only to indicate programmer's error, unproper API usage, don't use it for real-world errors
* Use `NSAssert/NSParameterAssert` whenever possible
* Indicate undesired behaviour by taking reference to `NSError **` and by returning `nil` or `NO` value with the method (synchronous methods) or with the completion block, by returning success status value and separate `NSError` object


```
- (BOOL)synchronousMethodThatDoesSomething:(NSError **)error{
    // some code
   ...
    // we got a problem
    if(somethingWentWrong){
        *error = [NSError errorWith...]
         return NO;
    }

    // everything was ok
    return YES
}

- (void)asynchronousMethodThatDoesSomethingWithCompletion:(void (^)(BOOL success, NSError* error, ...))completion{
    // some code
   ...
    // we got a problem
    if(somethingWentWrong){
        if(completion != nil){
           completion(NO, [NSError errorWith...], ...);
        }
        return;
    }

    // everything was ok
    if(completion != nil){
        completion(YES, nil, ...);
    }
}
```


##Other Coding Styles Guides
---

* [NYTimes](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)* 