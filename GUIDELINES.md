# CA Technologies Mobile App Service Objective-C Style Guide

This documentation introduces and covers the Objective-C coding style of iOS team at CA Technologies Mobile App Service team.  This guideline is recommended to obey in all Objective-C implementation of our products.

## Additional Reference

### Apple's Official Coding Guideline

On top of the guidelines that we define in this document, we would also recommend to go through the following Apple's official coding guideline as well.  

* [Programming with Objective-C](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [App Programming Guide for iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

### CA Technologies Mobile App Service

For more information about our product, please refer to official [Mobile App Service developer](http://mas.ca.com) site.

### List of MAS Products

Mobile App Service consists of multiple functional products separated into individual frameworks.

* MASFoundation
* MASUI
* MASConnecta
* MASIdentityManagement
* MASStorage

All of frameworks is recommended to obey this coding guideline.

## Table of Contents

* [Documentation](#documentation)
* [Pragma Mark](#pragma-mark)
  * [Commonly Used Pragma Mark](#commonly-used-pragma-mark)
  * [Pragma Mark syntax](#pragma-mark-syntax)
  * [Order of Pragma Mark](#order-of-pragma-mark)
* [Class Naming](#class-naming)
* [Imports](#imports)
* [Constants](#constants)
* [Spacing / Newline](#spacing-newline)
  * [Documentation / In-line Comment](#documentation-in-line-comment)
  * [Property / Enum](#property-enum)
  * [Method](#method)
  * [If/Switch/For and other statements](#if-switch-for-and-other-statements)
* [Nullability for Swift](#nullability-for-swift)
* [Project Configuration Guideline](#project-configuration-guideline)
  * [File Structure](#file-structure)
  * [Class Naming](#class-naming)
  * [Framework Error Handling](#framework-error-handling)

## Documentation


All of the code **should be properly documented** in `.h` and `.m` files. Code documentation should adhere Apple Doc's format.

As a team, we are using a tool that automatically generates document format in Xcode called [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode) available through [Alcatraz](http://alcatraz.io/).

Please install [Alcatraz](http://alcatraz.io/) and [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode), this tool will automatically generate the proper documentation format for method, property, class, and any other form of section required documentation.

Elements that should be documented are as follows:

* Class description
* Property
* Enum type
* Constant
* Method


For private methods that are not available in header file, the method should still be documented in implementation file if it is only in `.m` file.

## Pragma Mark

`# pragma mark - ` is used to categorize methods or properties in header and implementation files in Xcode.  It is used to make the codes easier to organize and to be navigated by other developers.

### Commonly Used Pragma Mark

There are few commonly used `# pragma mark` in MASFoundation.  Please try to follow similar set of function categories.

```objc
# pragma mark - Properties
# pragma mark - Lifecycle
# pragma mark - Public
# pragma mark - Private
```

These are commonly used `# pragma mark - ` in MASFoundation.  Any other functional group of methods is ok as long as it makes sense to everyone.

**For example:**

In `MASDevice.h`, methods that are specifically related to BLE functionalities, we may group those methods in `# pragma mark - Bluetooth Central/Peripheral`.

### Pragma Mark syntax

Please follow standard format of `pragma mark` that is also compatible with Apple Doc.

**For example:**

```objc
///--------------------------------------
/// @name Properties
///--------------------------------------

# pragma mark - Properties
```

**Not:**

```objc
#pragma mark - 
#pragma mark - Properties
```

* Note that you want to allow one space between # and pragma mark.

### Organization of Pragma Mark

Order of `# pragma mark` matters.  Place all of those commonly used `# pragma mark` at the top, and others followed.  
Order of `# pragma mark` should be identical in both header and implementation files.

**All of the methods** under `# pragma mark` should be in **alphabetical order**.


## Imports

* Imports might be in alphabetical order
* Always make use of foward declaration in the interface (.h) file. I.e. `@class MASObject` instead of `#import "MASObject.h"`

**For example:**

```objc
//
// All imported classes should be in alphabetical order
//
#import <MASFoundation/MASApplication.h>
#import <MASFoundation/MASAuthenticationProvider.h>
#import <MASFoundation/MASAuthenticationProviders.h>
#import <MASFoundation/MASConfiguration.h>
#import <MASFoundation/MASDevice.h>
#import <MASFoundation/MASFile.h>
#import <MASFoundation/MASGroup.h>
#import <MASFoundation/MASObject.h>
#import <MASFoundation/MASUser.h>
#import <MASFoundation/MASSessionSharingQRCode.h>
#import <MASFoundation/MASMessage.h>
```

**Not:**

```objc
#import <MASFoundation/MASObject.h>
#import <MASFoundation/MASAuthenticationProviders.h>
#import <MASFoundation/MASAuthenticationProvider.h>
#import <MASFoundation/MASConfiguration.h>
#import <MASFoundation/MASMessage.h>
#import <MASFoundation/MASDevice.h>
#import <MASFoundation/MASFile.h>
#import <MASFoundation/MASApplication.h>
#import <MASFoundation/MASGroup.h>
#import <MASFoundation/MASUser.h>
#import <MASFoundation/MASSessionSharingQRCode.h>
```

## Constants

Constants are meant to be used for easy reproduction of commonly used values in multiple places in a same or multiple classes and also for avoiding a situation inserting hard coded values in implementation file.
Any contant values that have to be publicly available, place constants in `MASConstant.h` file, otherwise, place constants in `MASConstantsPrivate.h` files.

For constants' syntax, always declare the variable as `static` constant and avoid using `#define` unless otherwise it's meant to be used as a macro.

**For example:**

```objc
static NSString *const MASPublicConstantValue = @"MASPublicConstantValue";
```

**Not:**

```objc
#define MASAvoidUsingThisConstant @"MASAvoidUsingThisConstant"
```


## Spacing / Newline

* All of the spacing format should be consistent across all places in the code.
* Indentation should be using 4 spaces.  Please make sure in `Xcode's preference > Text Editing > Indentation`, Tab and indentation are 4 spaces (default value).


### Documentation / In-line Comment

#### Documentation
* For method, property, and class documentation, please use [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode) to make it consistent for all documentation style.

**For example:**
* Method

```objc
/**
 *  Sets the MASDeviceRegistrationType property.  The default is MASDeviceRegistrationTypeClientCredentials.
 *
 *  @param registrationType The MASDeviceRegistrationType.
 */
+ (void)setDeviceRegistrationType:(MASDeviceRegistrationType)registrationType;
```

* Property

```objc
/**
 *  The class name of the object.
 */
@property (nonatomic, readonly, copy) NSString *className;
```

#### In-line Comment

* Place an in-line comment right above the line of codes
* In-line comment should be added to help understand the code easily by other developers
* There should be leading and trailing empty in-line comment, `//`
* Insert **1 space** in between the comments and in-line comment section

**For example:**

```objc
//
// Place an in-line comment like this
//
```

**Not:**

```objc
//Don't do in-line comment like this
```

### Property / Enum

#### Spacing
* Space should be added for each word and `*` and `property name` should not have a space.

**For examples:**

* Property

```objc
@property (nonatomic, copy, readwrite) NSString *userName;
```

* Enum

```objc
typedef NS_ENUM(NSInteger, MASDeviceRegistrationType)
```


**Not:**

```objc
@property(nonatomic, copy,readwrite) NSString*userName;
```

#### Newline
* There should be __2 new lines__ in between each property declaration and **1 new line** for the first property in `pragma mark section` in `.h` file along with proper documentation for the description of the property.

**For examples:**

```objc
///--------------------------------------
/// @name Managing Object Properties
///--------------------------------------

# pragma mark - Managing Object Properties

/**
 *  The class name of the object.
 */
@property (nonatomic, readonly, copy) NSString *className;


/**
 *  The id of the object.
 */
@property (nonatomic, readonly, copy) NSString *objectId;
```

```objc
///--------------------------------------
/// @name MAS Constants
///--------------------------------------

# pragma mark - MAS Constants

/**
 * The enumerated MASRegistrationTypes.
 */
typedef NS_ENUM(NSInteger, MASDeviceRegistrationType)
{
    /**
     * Unknown encoding type.
     */
    MASDeviceRegistrationTypeUnknown = -1,
    
    /**
     * The client credentials registration type.
     */
    MASDeviceRegistrationTypeClientCredentials,
...
...
};


/**
 * The enumerated MASRequestResponseTypes that can indicate what data format is expected
 * in a request or a response.
 */
typedef NS_ENUM(NSInteger, MASRequestResponseType)
{
...
...
```

**Not:**

```objc
///--------------------------------------
/// @name Managing Object Properties
///--------------------------------------
# pragma mark - Managing Object Properties
/**
 *  The class name of the object.
 */
@property (nonatomic, readonly, copy) NSString *className;
/**
 *  The id of the object.
 */
@property (nonatomic, readonly, copy) NSString *objectId;
```

### Method

#### Spacing
* There should be **1 space** after the method scope and in between the method segments.
* There should also be **1 space** in between `object type` and `*` for every parameter and return type of the method if needed.

**For example:**

```objc
+ (void)start:(MASCompletionErrorBlock)completion;

+ (MASObject *)currentObject;

- (void)setObject:(id)object forKeyedSubscript:(id <NSCopying>)key;
``` 

**Not:**

```objc
+ (void)start: (MASCompletionErrorBlock)completion;

+(MASObject*)currentObject;

-(void)setObject:(id)object forKeyedSubscript:(id<NSCopying>)key;
``` 

#### Newline

* There should be __3 new lines__ in between each method declarations and **1 new line** for the first property in `pragma mark section` in `.h` file.
* There should be __2 new lines__ in between each method declarations and **1 new line** for the first property in `pragma mark section` in `.m` file.
* Opening and closing braces for method implementation should always be in newline.
* All private method that are not present in `.h` file should also be documented in `.m` file with same documentation format.

**For example: (.h file)**

```objc
///--------------------------------------
/// @name Accessors
///--------------------------------------

# pragma mark - Accessors

/**
 *  Returns the value associated with a given key.
 *
 *  @param key The given identifying key for which to return the corresponding value.
 *
 *  @return The value associated with a given key.
 */
- (id)objectForKey:(id)key;



/**
 *  Sets the object associated with a given key.
 *
 *  @param object The object for `key`. A strong reference to the object is maintaned by MASObject.
 *                Raises an `NSInvalidArgumentException` if `object` is `nil`.
 *                If you need to represent a `nil` value - use `NSNull`.
 *
 *  @param key    The key for `object`. Raises an `NSInvalidArgumentException` if `key` is `nil`.
 */
- (void)setObject:(id)object forKey:(id <NSCopying>)key;
```

**For example: (.m file)**

```objc
#pragma mark - pragma section

- (id)objectForKey:(id)key
{
...
    return object;
}


- (void)setObject:(id)object forKey:(id <NSCopying>)key
{
...
}
```


### If/Switch/For and other statements

#### Spacing / Newline

* There should be **1 space** in between control statement and bracket as well as properties and operator.
* Opening brace should always be in new line for first `if` statement, other `else` or `else if`'s opening brace should be placed in the same line.
* Closing brace should always be in new line.
* Opening and closing brace should always be included even for one line of code in the statement.

**For example:**

```objc
if (isBoolean) 
{
...
}
else {

}

if (isBoolean && isNotBoolean && [[MASObject currentObject] isBoolean]) 
{
...
}
else if (otherBoolean) {
...
}

if (isBoolean)
{
  counter++;
}
```

```objc
switch (type) {
    //
    // Configuration
    //
    case MASSwitchValue:
        value = @"value";
        break;
            
    //
    // Default
    //
    default: 
      value = @"default";
      break;
}

for (NSString *stringObj in objects)
{
...
}
```

**Not:**

```objc
if(isBoolean){
...
}else{

}

if (isBoolean && isNotBoolean&& [[MASObject currentObject]isBoolean]){
...
} else if(otherBoolean) 
{
...
}

if (isBoolean) counter++;

```

```objc
switch(type){
    case MASSwitchValue:
        value = @"value";
        break;
            
    default: 
      value = @"default";
      break;
}

for(NSString *stringObj in objects){
...
}
```

## Nullability for Swift

As we are aiming to support Swift with bridge header, please be aware of Nullability while writing codes in Objective-C.  In Objective-C, Nullability does not make any differece, but it will make a difference when developers are using our framework in Swift.

Please properly use `_Nullable` and `_Nonnull` type annotation in properties and methods.

**For example:**

```objc
@property (copy, nullable) NSString *name;
@property (copy, readonly, nonnull) NSDictionary *thisDictionary;
```

For more detail, please review [this blog post](https://developer.apple.com/swift/blog/?id=25)

## Project Configuration Guideline

As we are maintaining multiple frameworks, please ensure that all framework should adhere same style, format, and file structure.

### File Structure

For any new files or folder (groups in xCode), please adhere the following folder structure.

```objc
- Framework name folder (i.e. MASFoundation, MASUI, MASConnecta)
-- Framework public header file
-- Class (Folder)
--- categories
--- models
--- _private_
---- categories
---- services
---- models
-- Vender
```

* All public files should be located under `Classes`, `categories (under Classes)` and `models (under Classes)`.
* All private files should be located under `categories`, `services`, `models`, or any other folders under `_private` folder.
* When making a new group in Xcode, please make sure to create an **actual directory** in file system, and add the directory into project as group.
* Any third-party libraries being used, please include them in `Vendor` folder.
* It is recommended to rename the prefix of third-party libraries in order to avoid any conflict with other developers' project which may use the same library.
* Please always ensure the license of the third-party library and consult it with other team member if it is allowed to use.

### Class Naming


* All classes should start with prefix, `MAS`, except for category class from iOS SDK or other vendors.

#### All frameworks
* For newly created model class, the class should be inherited from `MASObject.h`.
* For newly created service class, the class should be inherited from `MASService.h`.
* For class used in two or more frameworks, a model of the class must be created in `MASFoundation` and a category of the class in the specific framework.

#### MASService

When you create a new service class, make sure to implement service class inherited from `MASService`.

* Please make sure to define a constant in `MASConstantsPrivate.h` class with uniquely generated UUID.
* When you are creating service class in external framework outside of `MASFoundation`, please make sure to include the UUID in `MASConstantsPrivate.h` and use the **same** UUID value in external service class' `+ (NSString *)serviceUUID` property.

**For example:**

```objc
+ (NSString *)serviceUUID
{
    // DO NOT change this without a corresponding change in MASFoundation
    return @"8b66aaa4-efbf-11e5-9ce9-5e5517507c66";
}
```

#### MASUI
* For newly created `viewController` class, the class should be inherited from `MASViewController.h`.

#### Category Class
For public category classes, name it with the framework name. If the category is not public, name it with the framework name and the word `Private` in the end. Privates categories must be created under the `_private_` folder.  

**For example:**

```objc
// For public category class
@interface NSData (MASFoundation)

// For private category class. Must reside inside the _private_ folder
@interface MASUser (MASFoundationPrivate)
```

### Framework Error Handling

All framework handles the error with proper framework error domains.

In MASConstants, or any framework constant file, properly define the error domain constants.  The error domain format should be **com.ca.FRAMEWORKNAME:SUB_DOMAIN**.

**For example:**

```objc
// The NSString error domain used by all MAS server related Foundation level NSErrors.
static NSString *const MASFoundationErrorDomain = @"com.ca.MASFoundation:ErrorDomain";

// The NSString error domain used by all MAS local level NSErrors.
static NSString *const MASFoundationErrorDomainLocal = @"com.ca.MASFoundation.localError:ErrorDomain";

// The NSString error domain used by all target API level NSErrors.
static NSString *const MASFoundationErrorDomainTargetAPI = @"com.ca.MASFoundation.targetAPI:ErrorDomain";
```
