# The Dojo Toolkit

The Dojo Toolkit is a non-prescriptive collection of JavaScript modules
designed to work together to help you build well-architected, high-performance
Web applications.

## This repository

This repository is an experimental repository for the next major version of the
Dojo Toolkit.

## Code conventions

Code committed to this repository should follow these jshint rules:

```
asi=false bitwise=false boss=false browser=true camelcase=true couch=false
curly=true debug=false devel=true dojo=false eqeqeq=true eqnull=true es5=true
esnext=false evil=false expr=true forin=false funcscope=true globalstrict=false
immed=true iterator=false jquery=false lastsemic=false latedef=false
laxbreak=true laxcomma=false loopfunc=true mootools=false multistr=false
newcap=true noarg=true node=false noempty=false nonew=true nonstandard=false
nomen=false onecase=false onevar=false passfail=false plusplus=false proto=false
prototypejs=false regexdash=true regexp=false rhino=false undef=true
unused=true scripturl=true shadow=false smarttabs=true strict=false sub=false
supernew=false trailing=true validthis=true withstmt=false white=true
worker=false wsh=false yui=false indent=4 predef=["require","define"]
quotmark=single maxcomplexity=10
```

Unless otherwise noted, these options must not be overridden using `.jshintrc`
or `/*jshint*/`.

`else` keywords must be on their own line, not cuddled with the closing
bracket of the previous block. This is consistent with the use of all other
block statements.

`var` declarations that declare multiple variables at once must always put
each variable identifier on its own line. This prevents variable declarations
being lost inside long lists that may also include immediate assignments.

All public APIs must be commented using jsdoc annotations.

### Rationales

* `asi`: Relying on ASI is sloppy and leads to inadvertent code breakage in
  edge cases. Using semicolons only in these edge cases requires all authors
  to be aware of them and means semicolons are used inconsistently.
* `bitwise`: Bitwise operators are useful in a wide variety of code and should
  not generally be restricted. Code reviews ensure bitwise operators are not
  accidentally used instead of logical operators.
* `boss`: Intentional assignment within conditionals should be done by wrapping
  the assignment in an extra set of parens.
* `browser`: Most Dojo Toolkit code is designed to run within browser
  environments, so predefining browser globals is desirable. Code that should
  never run within a browser environment should use
  `/*jshint browser:false */`.
* `camelcase`: This option is enabled to ensure consistent variable naming
  conventions that match those defined by the language.
* `couch`: Dojo Toolkit code is not usually designed to run within CouchDB
  environments. This may be set true with a `/*jshint*/` comment for files
  designed to run within CouchDB.
* `curly`: This options is enabled to ensure codebase consistency.
* `debug`: This option is disabled because no code should be committed to the
  repository with debugger statements.
* `devel`: This option is enabled because all platforms supported by the
  toolkit have the global `console` object, and the build system can strip
  console calls for production deployments. `console` methods are used to
  inform developers that they may not be using the toolkit properly.
* `dojo`: This option is disabled because it defines global `dojo`, `dijit`,
  and `dojox` objects, but these objects should never be used by new code.
  The two globals used by the toolkit, `define` and `require`, are defined
  in the `predef` array.
* `eqeqeq`: Even extremely experienced JavaScript programmers do not understand
  all the type coercion rules of JavaScript, which leads to subtle programming
  errors in edge cases. Additionally, non-strict equality can sometimes lead to
  confusion as to what types of input are intended to be accepted by a given
  statement. If type coercion is desired when performing a direct equality
  check, it should be done manually (e.g. `"" + foo === "" + bar`).
* `eqnull`: Non-strict equality to `null` may still be performed as this is the
  cleanest way to check whether a value is either `null` or `undefined`.
* `es5`: All code in version 2 of the toolkit runs only in EcmaScript 5
  environments.
* `esnext`: Code in version 2 of the toolkit should not use ES6 syntaxes.
* `evil`: In order to discourage the incorrect use of `eval` in codebases that
  follow the Dojo code conventions, this option is false by default. However,
  there are situations where the use of `eval` is necessary. In these cases,
  this option may be set to true using `/*jshint*/`.
* `expr`: The use of an expression statement to conditionally evaluate code is
  often clearer than wrapping the code inside a true conditional.
* `forin`: The toolkit requires an ES5 environment, which means users may
  augment `Object.prototype` with non-enumerable properties without breaking
  for-in loops. Therefore, there is no reason to filter these loops.
* `funcscope`: When authoring code, declaring variables as close to the point
  where they are first used is preferred as it helps to cluster groups of code
  together instead of spreading them throughout a function.
* `globalstrict`: Global `"use strict"` breaks third-party code. Since Dojo
  code is never executed in global scope, this should not matter, but is
  provided for completeness.
* `immed`: Immediately invoked function expressions should be wrapped in parens
  for consistency and to ensure that readers of the code understand that the
  value being assigned is the output of the function and not the function
  itself.
* `iterator`: The use of non-standard JavaScript engine properties violates
  cross-platform compatibility and future-proofness, so is forbidden.
* `jquery`: Dojo Toolkit does not access global objects of other libraries.
* `lastsemic`: For consistency, semicolons are required at the end of all
  statements.
* `latedef`: Define after use is a very common pattern that occurs when
  two functions defined within a scope reference one-another through closure.
* `laxbreak`: This has caused enough false positives in the past that it is
  currently disabled.
* `laxcomma`: Comma-first coding style is forbidden by the Dojo style
  guidelines.
* `loopfunc`: XXX rationale
* `mootools`: Dojo Toolkit does not access global objects of other libraries.
* `multistr`: The use of multi-line string syntax virtually always results in
  incorrect code indentation, so is disallowed.
* `newcap`: An uppercase first letter is the only way to notate an object as
  being a constructor that requires the `new` keyword, so this convention is
  enforced.
* `noarg`: In order to discourage the incorrect use of `arguments` in codebases
  that follow the Dojo code conventions, this option is false by default.
  However, there are situations where the use of `arguments.callee` is
  necessary. In these cases, this option may be set to false using
  `/*jshint*/`.
* `node`: The majority of toolkit code is designed to run in the browser, so
  this option is false by default to avoid accidental use of Node.js globals
  in the most common cases. Files that are designed to run in Node.js and
  access its global objects may set this to true using `/*jshint*/`.
* `noempty`: Empty blocks are occasionally useful when iterating using `while`
  or when writing complex conditionals to be easier to understand, so are
  allowed. In the case of conditionals, it is recommended that a comment
  `// do nothing` be added to the empty block to make clear it is intended to
  do nothing.
* `nonew`: XXX rationale
* `nonstandard`: The only two non-standard globals are `escape` and `unescape`;
  the standard `encodeURIComponent` and `decodeURIComponent` should be used
  instead.
* `nomen`: For consistency, dangling underscores at the ends of variables are
  disallowed.
* `onecase`: A switch with one case should be written as an if or if/else
  statement for consistency. *n.b. This option is obsolete in jshint.*
* `onevar`: Same rationale as `funcscope`.
* `passfail`: Stopping at one error simply obfuscates other errors.
* `plusplus`: Unary increment/decrement operators are extremely useful,
  widely used, and widely understood.
* `proto`: The `__proto__` object is not available on all supported platforms,
  so may not be used.
* `prototypejs`: Dojo Toolkit does not access global objects of other
  libraries.
* `regexdash`: Unescaped dash at the end of a character group is common and
  harmless. *n.b. This option is obsolete in jshint.*
* `regexp`: Dot in regular expressions is common and harmless. *n.b. This
  option is obsolete in jshint.*
* `rhino`: Dojo Toolkit code is not usually designed to run within Rhino
  environments. This may be set true with a `/*jshint*/` comment for files
  designed to run within Rhino.
* `undef`: This option prevents the accidental creation or use of global
  variables.
* `unused`: This option prevents the accidental creation of unused variables.
* `scripturl`: XXX rationale
* `shadow`: Variable shadowing is common and is preferable to generating new
  verbose identifier names just to avoid shadowing, especially in cases where
  variables from the closure are not used.
* `smarttabs`: While the toolkit uses hard tabs for indentation, it is
  still important to be able to sometimes align sections of indented code
  precisely using spaces to improve readability. Additionally, jshint has
  historically triggered on code comments that mix spaces and tabs, which is
  not desirable in any scenario.
* `strict`: Strict mode is not supported across all platforms supported by the
  toolkit, which means that code will execute differently on different
  platforms if `"use strict"` is enabled. Furthermore, certain frameworks like
  .NET walk call chains using `arguments.callee` and will break if any function
  within the call chain uses `"use strict"`. Since strict mode provides no
  substantial benefit when code is already being passed through a linter, its
  use within the toolkit is forbidden.
* `sub`: Rarely, array notation is used instead of dot notation to prevent
  build tools from processing or mangling certain sections of code. This should
  be extremely uncommon in new toolkit code, however, so the more efficient dot
  notation is preferred for the sake of consistency. In a situation where array
  notation is necessary, this option may be set to true using `/*jshint*/`.
* `supernew`: As the toolkit uses AMD modules, the creation of singletons
  will generally never require the use of `new function` syntax. However, if
  a situation arises where this is necessary, this option may be set to true
  using `/*jshint*/`.
* `trailing`: Trailing whitespace makes the code look unprofessional.
* `validthis`: Since strict mode is verboten, the value of this setting does
  not matter, so it defaults to `true` (the default for jshint).
* `withstmt`: The `with` statement prevents minification of code blocks and
  ruins JIT performance and so is verboten.
* `white`: Because the toolkit is written by a wide variety of contributors, a
  mechanism for programmatically ensuring whitespace consistency is strongly
  needed in order to ensure that the codebase looks and feels cohesive. This is
  the only mechanism for ensuring whitespace consistency that is available, and
  its rules are not unreasonable. Therefore, it is enabled.
* `worker`: Dojo Toolkit code is not usually designed to run within Web Worker
  environments. This may be set true with a `/*jshint*/` comment for files
  designed to run within Web Workers.
* `wsh`: Dojo Toolkit code is not usually designed to run within WSH
  environments. This may be set true with a `/*jshint*/` comment for files
  designed to run within WSH.
* `yui`: Dojo Toolkit does not access global objects of other libraries.
* `indent`: Same rationale as `white`. An indent value of 4 is enforced because
  it is the most commonly used indent size. Since the toolkit uses hard tabs,
  you may set your editor to whatever indent size you prefer, but jshint will
  always assume that one tab equals four spaces for the purposes of code
  validation.
* `predef`: The only global variables that toolkit code should use are
  `require` and `define`.
* `quotmark`: Same rationale as `white`. A single quote is enforced because it
  requires one fewer keystroke on standard QWERTY keyboards.
* `maxcomplexity`: In order to avoid creating massive, unmaintainable
  functions, a default cyclomatic complexity limit of 10 is enforced on all
  toolkit code. If an individual function can justify an increased complexity
  limit, `/*jshint*/` may be used within the function to increase the limit as
  long as a comment justifying the increase is also provided. Refactoring code
  that violates the complexity limit is preferred.

## License

Licensed under the terms of the [New BSD license](LICENSE).