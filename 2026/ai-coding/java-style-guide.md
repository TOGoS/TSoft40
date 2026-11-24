# Java Style

Copied from [Scratch38's JAVASTYLE.md](https://github.com/TOGoS/Scratch38/blob/ac07b78dd5663f48c257bbf0e9a5ed81833d0e1c/JAVASTYLE.md).

Try to apply these formatting rules to new code.
Do not reformat existing code unless it is being changed for other reasons,
for consistency with other lexically-nearby changes,
or because the architect specifically asked for reformatting.

Do reformat existing source files that are being significantly rewritten anyway
(as a rule of thumb, let's say: when more than about half the lines changed).

Ideally formatting would be done mechanically by a formatting tool.
As of this writing we don't yet have one in place.

## Identifier naming

- For new identifiers, prefer explicit names that describe what the value actually represents,
  especially if a generic type (such as String) is used for the representation. e.g.
  - `fooPath` or `fooUri` rather than `foo` for a string that references a file by filesystem path or URI
  - `fooId` or `fooCode` for database columns containing the primary key of a foo
    ('ID' being more general; 'code' is sometimes used for short alphanumeric names)
- When using `Map`s to represent objects, an `info` suffix is often appropriate.
  - e.g. `Map<String,Object> makeErrorInfo(...)`
  - This pattern is common in health/deployment info endpoints
    in cases where we elide more strongly-typed representations for one-off bits of detail,
    since the JSON-encoding functions we use operate on `Map`s and `List`s.
- Omit qualifiers only when the surrounding context or convention already makes their meaning clear,
  e.g. a list of constants inside a `FooPaths` class may omit a `_PATH` suffix that would
  otherwise be expected.
- Fields that directly map to database columns may keep the database column name,
  converted to camel case, e.g. `USR_BNK_CD` may be `usrBnkCd`.  `userBankCode` would
  also be acceptable if that meaning is understood and all fields are named similarly.

## Identifier formatting

In new code, use the convention described by [TOGoS](http://www.nuke24.net/docs/2018/CamelCase.html).

`UpperCamelCase` for class names and so on.
Rule: Upper-case the first letter of each word, then smash the words together.
Implication: acronyms stay capitalized.

`lowerCamelCase` for variables, method names, and so on.
Rule: lower-case all words, upper-case the first letter of all but the first, then smash them together.
Implication: acronyms become lower-case except for the first letter when not the first word.

For constants, upper-case and join with underscores.  This is the usual convention.

Examples:
- "XML HTTP request" (english) would become `XMLHTTPRequest` (class name), or `xmlHttpRequest`
- "ID of user's XML document" (not a name we'd actually use, but just for examples' sake)
  would become `IDOfUsersXMLDocument` as a type name and `idOfUsersXmlDocument` as a field/variable name.

This convention avoids problems that other conventions suffer from,
such as inconsistent casing of acronyms (`idVsID`) or butt-ugliness (`iDOfMyDog`).

## Class member definition order

Generally:
- Static stuff first
- Instance variables
- Constructors
- Instance methods

Within those categories, methods should be defined low-level-to-high-level.
Try to define methods before they are referenced by other methods.
The outermost methods, e.g. those that implement an interface,
`main`, or servlet request handlers, will appear last.

## Formatting

- Use Unix-style line endings for new text files
  (unless writing a format that requires something else).
- Use tabs for indentation in Java source files.
- Use spaces only for alignment after tabs if needed.
- Preserve indentation on blank lines inside blocks; a blank line should still carry the block's indent.
- The machine-readable form of these settings, when it exists, is a
  `.editorconfig` at the root of the repository whose code you are
  editing.  Repositories following this guide usually have one, but
  this docs project does not; there is nothing to link to here.

### Indentation

Structures that easily fit on one line should be put on one line.
'Easily' depends on context, where the choice should be about 70%
a reflection of the complexity of the code and 30% actual character count.

For example:
- Methods, especially constructors, that take more than one argument may give each argument its own line
  'for clarity'
- Trivial getters should usually be reduced to a single line,
  since that line contains a lot of redundant information.

There is no perfect way to format throws clauses;
just try to avoid excessive indentation.

```
class Foo<T>
implements Foobricator
{
	protected final String name;
	protected final T thingy;
	
	public Foo(
		// Note ONE additional level of indent here, not two.
		String name,
		T thingy
	) {
		this.name = name;
		this.thingy = thingy;
	}
	
	public T getThingy(T thingy) { return this.thingy; }

	public void doSomethingThatMightThrowBar(
		int bar
	) throws CraptacularException { // throws may be 'cuddled' after a multi-line parameter list
		,,,
	}
	
	public void doSomethingThatMightThrowFoo()
	throws CraptacularException // throws on its own line when not 'cuddled'
	{
		,,,
	}
}
```

One tab per indent.  This applies to regular code blocks, parameter lists, and array contents.
