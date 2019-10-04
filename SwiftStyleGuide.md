# **Swift Style Guide**
## Code Formatting
- Use 4 spaces for tabs.
- Avoid uncomfortably long lines with a hard maximum of 150 characters per line (Xcode->Preferences->Text Editing->Page guide at column: 150 is helpful for this)
- Ensure that there is a newline at the end of every file.
- Ensure that there is no trailing whitespace anywhere (Xcode->Preferences->Text Editing->Automatically trim trailing whitespace + Including whitespace-only lines).
- Do not place opening braces on new lines.
- When writing a type for a property, constant, variable, a key for a dictionary, a function argument, a protocol conformance, or a superclass, don't add a space before the colon.
- In general, there should be a space following a comma.
- There should be a space before and after a binary operator such as +, ==, or ->. There should also not be a space after a ( and before a ).
- When calling a function that has many parameters, put each argument on a separate line with a single extra indentation.
- Prefer using local constants or other mitigation techniques to avoid multi-line predicates where reasonable.
- Files should be logically organized by using `// MARK: -` and `// MARK:` statements. Below are some examples:
```swift
class TypicalViewcontroller.swift {

  // properties...
	
  // MARK: - Lifecycle
	
  // viewDidLoad, viewWillAppear etc.
	
  // MARK: - Actions
	
  // IBActions and @objc methods
	
  // MARK: - Private
	
  // general private methods
 }

  // MARK: - ProtocolName

 extension TypicalViewcontroller: ProtocolName {
 
  // implementation
	
 }
```
 ```swift
class TypicalPresenter.swift {
	
  // properties...
	
  // MARK: - Lifecycle
	
  // initializers
	
  // MARK: - Private
	
  // general private methods
	
  // MARK: Network
	
  // private methods dealing with network requests
	
  // MARK: Data
	
  // private methods responsible for working with data
 }

  // MARK: - ProtocolName

 extension TypicalPresenter: ProtocolName {
	 
  // implementation
	 
 }
```
## Naming
- There is no need for Objective-C style prefixing in Swift.
- Use `PascalCase` for type names (e.g. `struct`, `enum`, `class`, `typedef`, `associatedtype`, etc.).
- Use `camelCase` (initial lowercase letter) for function, method, property, constant, variable, argument names, enum cases, etc.
- Separate words with dashes for Assets names (`next_button_selected`), with dashes and dots in Localizable.strings file (`“text_field.placeholder.first_name”`)
- When dealing with an acronym or other name that is usually written in all caps, actually use all caps in any names that use this in code. The exception is if this word is at the start of a name that needs to start with lowercase - in this case, use all lowercase for the acronym (`HTTPResponse`, `urlBuilder`).
- For generics and associated types, use a `PascalCase` word that describes the generic. If this word clashes with a protocol that it conforms to or a superclass that it subclasses, you can append a `Type` suffix to the associated type or generic name.
- Names should be descriptive and unambiguous.
- Do not abbreviate, use shortened names, or single letter names.
- Include type information in constant or variable names when it is not obvious otherwise. Always include type information in IBOutlets names, unless it is not reasonable.
- When naming function arguments, make sure that the function can be read easily to understand the purpose of each argument. Also pay attention to how function reads on the call site.
- As per [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/ "Apple's API Design Guidelines"), protocols should be named as nouns if they describe what something is doing (e.g. `Collection`) and using the suffixes `able`, `ible`, or `ing` if it describes a capability (e.g. `Equatable`, `ProgressReporting`). If neither of those options makes sense for your use case, you can add a `Protocol` suffix to the protocol's name as well. 
- IBActions and @objc actions (passed as selectors) should (generally) end with word `Action` (e.g. `loginAction`, `nextAction`).
## Coding Style
### General
- Prefer the composition of `map`, `filter`, `reduce`, etc. over iterating when transforming from one collection to another. Make sure to avoid using closures that have side effects when using these methods.
- Prefer not declaring types for constants or variables if they can be inferred anyway.
- If a function returns multiple values, prefer returning a tuple to using `inout` arguments (it’s best to use labeled tuples for clarity on what you’re returning if it is not otherwise obvious). If you use a certain tuple more than once, consider using a typealias. If you’re returning 3 or more items in a tuple, consider using a struct or class instead.
- Be wary of retain cycles when creating delegates/protocols for your classes; typically, these properties should be declared `weak`.
- Be careful when calling `self` directly from an escaping closure as this can cause a retain cycle - use a capture list when this might be the case.
- Don't use labeled breaks.
- Don't place parentheses around control flow predicates.
- Avoid writing out an enum type where possible - use shorthand.
```swift
// PREFERRED
imageView.setImageWithURL(url, type: .person)
// NOT PREFERRED
imageView.setImageWithURL(url, type: AsyncImageView.Type.person)
```
- Prefer not writing `self.` unless it is required.
- When using a statement such as `else`, `catch`, etc. that follows a block, put this keyword on the same line as the block. 
```swift
if someBoolean {
  // do something
} else {
  // do something else
}
```
- Keep in mind that even though Singleton pattern is not treated as an anti-pattern, it is better to avoid using it whenever possible/reasonable (via dependency injection and other techniques).
You can declare a singleton property as follows:
```swift
class NetworkManager {
  // MARK: - Singleton 
  static let shared = NetworkManager()
  private init() {}
}
```
- When checking if a collection is empty or not, prefer `isEmpty` to `.count == 0`
### Access Modifiers
- Write the access modifier keyword first if it is needed.
- The access modifier keyword should not be on a line by itself - keep it inline with what it is describing.
- In general, do not write the `internal` access modifier keyword since it is the default.
- Prefer `private` to `fileprivate` wherever possible.
- IBActions and @objc methods should be `private` whenever possible.
### Custom Operators
- Prefer creating named functions to custom operators.
- If you want to introduce a custom operator, make sure that you have a very good reason why you want to introduce a new operator into global scope as opposed to using some other construct.
- You can override existing operators to support new types (especially `==`). However, your new definitions must preserve the semantics of the operator. For example, `==` must always test equality and return a boolean.
### Switch Statements and enums
- When using a `switch` statement that has a finite set of possibilities (`enum`), do NOT include a `default` case. Instead, place unused cases at the bottom and use the `break` keyword to prevent execution.
- Since switch cases in Swift break by default, do not include the `break` keyword if it is not needed.
- The `case` statements should line up with the `switch` statement itself as per default Swift standards.
- When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types (e.g. `case hunger(let hungerLevel)` instead of `case hunger(Int)`). Make sure the label does not conflict with variables already existing in the scope, which can be the reason for unexpected behavior.
- Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible.
### Optionals
- The only time you should be using implicitly unwrapped optionals is with IBOutlets. In every other case, it is better to use a non-optional or regular optional property. Yes, there are cases in which you can probably "guarantee" that the property will never be `nil` when used, but it is better to be safe and consistent. Similarly, don't use force unwraps.
The only exception to this rule is unit tests, because crash is a desired behavior there.
- Don't use `as!` or `try!`.
- If you don't plan on actually using the value stored in an optional, but need to determine whether or not this value is `nil`, explicitly check this value against `nil` as opposed to using `if let` syntax.
- Don't use `unowned`. You can think of `unowned` as somewhat of an equivalent of a `weak` property that is implicitly unwrapped (though `unowned` has slight performance improvements on account of completely ignoring reference counting). Since we don't ever want to have implicit unwraps, we similarly don't want `unowned` properties.
- When unwrapping optionals, use the same name for the unwrapped constant or variable where appropriate.
### Protocols

When implementing protocols, there are three ways of organizing your code:
1. Using `// MARK: -` comments to separate your protocol implementation from the rest of your code
2. Using an extension outside your `class`/`struct`/`enum` implementation code, but in the same source file
3. Using an extension in another source file.

Keep in mind that when using an extension, however, the methods in the extension can't be overridden by a subclass, which can make testing difficult. If this is a common use case, it might be better to stick with method #1 for consistency. Otherwise, method #2 allows for cleaner separation of concerns.
Even when using method #2, add `// MARK:` statements anyway for easier readability in Xcode's method/property/class/etc. list UI.

Use method 3 when either 
- you are extending the type you don’t control (native SDK, third-party frameworks etc) or 
- new behavior does not have close relations with the existing one(s) and implementation requires writing a significant amount of code.  

### Properties
- Prefer `let` to `var` whenever possible.
- If making a read-only, computed property, provide the getter without the` get {}` around it.
- When using `get {}`, `set {}`, `willSet`, and `didSet`, indent these blocks.
- Though you can create a custom name for the new or old value for `willSet`/`didSet` and `set`, use the standard `newValue`/`oldValue` identifiers that are provided by default.
- When deciding between computed property and method, choose computed property **iff**
	- it makes sense linguistically (i.e. this API returns some form of information about an object or value’s state.)
	- computing its value has no side effects
	- time complexity is O(1)
	
 Choose method otherwise.

### Closures
- If the types of the parameters are obvious, it is OK to omit the type name, but being explicit is also OK. Sometimes readability is enhanced by adding clarifying detail and sometimes by taking repetitive parts away - use your best judgment and be consistent.
- If specifying a closure as a type, you don’t need to wrap it in parentheses unless it is required (e.g. if the type is optional or the closure is within another closure). 
- Keep parameter names on the same line as the opening brace for closures when possible without too much horizontal overflow (i.e. ensure lines are less than 150 characters).
- Use trailing closure syntax unless the meaning of the closure is not obvious without the parameter name. 
- Do not use trailing closure if the method has several closures as parameters. Each one shall be used explicitly.
### Arrays
- In general, avoid accessing an array directly with subscripts. When possible, use accessors such as `.first` or `.last`, which are optional and won’t crash. Prefer using a `for item in items` syntax when possible as opposed to something like `for i in 0 ..< items.count`. If you need to access an array subscript directly, make sure to do proper bounds checking. You can use `for (index, value) in items.enumerated()` to get both the index and the value.
- Never use the `+=` or `+` operator to append/concatenate to arrays. Instead, use `.append()` or `.append(contentsOf:)` as these are far more performant (at least with respect to compilation) in Swift's current state. If you are declaring an array that is based on other arrays and want to keep it immutable, instead of `let myNewArray = arr1 + arr2`, use `let myNewArray = [arr1, arr2].joined()`.
### Using guard Statements
- In general, we prefer to use an "early return" strategy where applicable as opposed to nesting code in `if` statements. Using `guard` statements for this use-case is often helpful and can improve the readability of the code.
- When unwrapping optionals, prefer `guard` statements as opposed to `if` statements to decrease the amount of nested indentation in your code.
- When deciding between using an `if` statement or a `guard` statement when unwrapping optionals is not involved, the most important thing to keep in mind is the readability of the code. There are many possible cases here, such as depending on two different booleans, a complicated logical statement involving multiple comparisons, etc., so in general, use your best judgement to write code that is readable and consistent. If you are unsure whether `guard` or `if` is more readable or they seem equally readable, prefer using `guard`.
- If choosing between two different states, it makes more sense to use an `if` statement as opposed to a `guard` statement.
- You should also use `guard` only if a failure should result in exiting the current context.
- Often, we can run into a situation in which we need to unwrap multiple optionals using `guard` statements. In general, combine unwraps into a single `guard` statement if handling the failure of each unwrap is identical (e.g. just a `return`, `break`, `continue`, `throw`).
- Use one-liners for single-check `guard` statements with single `return`, `break`, `continue`, `throw` expression.Use multiple lines in all other cases:
```swift
guard let result = result else { return }
guard let result = result else { 
  completion()
  return 
}
guard let json = result as? [String: AnyObject],
  let data = json[“data”] as? [String: AnyObject] else {
  return 
}
```
