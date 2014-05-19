#Objective-C Style Guide

The Objective-C Style Guide used by blinkbox books

These guidelines are built on Apple's existing [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html). 

## Table of Contents
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Prefixes](#prefixes)
* [Acronyms](#acronyms)
* [Whitespaces](#whitespaces)
* [Documentation](#documentation)
* [Organisation](#organisation)
* [Localisation](#localisation)
* [Errors and exceptions](#errors-and-exceptions)
* [Other Coding Styles Guides](#other-coding-styles-guides)

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

##Prefixes

All new classes and constants that are added to the project *must* be named with the prefix __BBB__ (from __B__link__B__ox__Books__) instead of old __BBBIosApp__ prefix.


All new methods added to the Cocoa/Foundation classes in categories *must* have __bbb__ prefix in their names.


##Acronyms
All methods and classes which have the `URL` acronym in their name, should spell it in uppercase letters. Methods and functions parameters can use all lowercase version if the name consists only of `url` word; if parameter name has more words in it, it must be spelled with `URL` in all uppercase. Example from [NSURLSession documentation](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/Introduction/Introduction.html):

```
- (NSURLSessionUploadTask *)uploadTaskWithRequest:(NSURLRequest *)request fromFile:(NSURL *)fileURL

- (NSURLSessionDataTask *)dataTaskWithURL:(NSURL *)url

- (NSURLSessionDataTask *)dataTaskWithURL:(NSURL *)url completionHandler:(void (^)(NSData *data, NSURLResponse *response, NSError *error))completionHandler

```


##Whitespaces

* Spaces or tabs?
* Braces should always open at the same line as the keyword `if,else,switch, for,while, method, functions` and close with the new line



```
if (someBooleanValue){
 // do stuff
}
else if(someOtherBooleanValue){
 // do something else
}
else{
 // do other things
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

* All public methods and classes should be documented using [AppleDoc](http://gentlebytes.com/appledoc/) style. You should surround Cocoa and your classes, values, method names with backticks (`) so when  Example method documenation: 


```
/**
 *  Designated initilizer method for `BBBSomethingViewController`
 *
 *  @param nibNameOrNil NIB file name or a `nil`
 *  @param animated     `BOOL` value (`YES` meaning with animation and `NO` means no animtion at all.
 *
 *  @return new instance of the `BBBSomethingViewController` or `nil` if something went wrong
 *
 *  @see `UIViewController`
 *  @see `NSDateFormatter`
 */
- (id) initWithNibName:(NSString *)nibNameOrNil booleanValue:(BOOL)animated{
    self = [self initWithNibName:nibNameOrNil
                      leftButton:animated];
    if(self != nil){
        [self setup];
    }
    return self;
}
```

We recommend using plugin for Xcode for putting new AppleDoc comments much faster - [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode), which can be installed through [Alcatraz](http://alcatraz.io), a Xcode plugin manager.


* Comments can be both single line (`//`) or block style (`/**/`)
* Use `#pragma mark - text` to organise code inside your implementation  file
* We strongly ecourage also documenting:
	* custom types (`NS_ENUM`,`NS_OPTIONS`) -  the type itself as well as each possible value,
	* C functions - functionality, parameters and returned value should be described in detail,
	* C macros

##Organisation

* Instance variables usage is prohibited outside custom `setters/getters` and `init/dealloc` methods
* Always use opening and closing braces for control structures (`if,else,switch, for,while`) even if there is only single statement inside it (we don't want another goto fail; )
* Usage of string literals in the code is discouraged, as well as numeric values. We should use const variables  whenever applicable.
* Subclasses of base Cocoa classes should have proper suffixes for easier finding, examples:
	1.  `UIViewController` subclasses should be called `BBB..........ViewController`,
	2.  `UIView` subclasses should be called `BBB...View`, 
	3.  `UITableViewCell` subclasses should be called `BBB...[TableView]Cell`
	
* Methods in the implementation file should be sorted and grouped  :
	1. Method overrides (from most root class to the highest lecel)
	2. Protocol methods
	
	```
	
	#pragma mark - NSObject
	- (id) init{
		...
	}
	- (void) dealloc{
		...
	}
	
	#pragma mark - UIViewController
	- (void) viewDidLoad{
		...
	}
	
	- (void) viewDidAppear{
		...
	}
	
	#pragma mark - Some protocol methods
	- (void) method{
	}
	
	```



##Localisation

* Never use string NSString literals for user-visible strings, always use NSLocalizedString() macro
* Use reverse DNS schema for user visible strings, this greatly improve ease of possible future localisation

``` 
label.text = NSLocalizedString(@"library-screen.button-title.main-menu",nil);
```

* When presenting date values to user, always use NSDateFormatter, NSDateComponent API
* For displaying content with flexible plurality or gender, we should use Localized Property List File (Localizable.stringsdict) format. [Unicode Refence](http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html#rules), [OSX 10.9 reference](https://developer.apple.com/library/Mac/releasenotes/Foundation/RN-Foundation/index.html)

##Errors and exceptions

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


* [NYTimes](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)