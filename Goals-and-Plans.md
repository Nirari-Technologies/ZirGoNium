# ZirGoNium

* Anonymous Functions.
	* capturing is done via scope-analysis and passing the variable by reference parameter.

* Statically typed w/ type inferencing.

* Run-time Type Information.

* Names starting with an underscore are private/static/file-only.

* Chainable comparison operators.

* App-defined Types can be exported without a glue interface (requires embeddable compiler).

* App-defined Types can define their own operators.
	* Operator Overloading (full operators from host app, limited for script devs)
		* script devs only get limited number of operators to overload.
		* script overloadable operators operators:
		```
			`<- (read)`        - func S method() T
			`<-2 (read)`       - func S method() (T1, T2)
			`-> (read)`        - func S method() T
			`->2 (read)`       - func S method() (T1, T2)
			`<-= (write)`      - func S method(a T)
			`->= (write)`      - func S method(a T)
			`[] (read)`        - func S method(a T1) T2
			`[]2 (read)`       - func S method(a T1) (T2, T3)
			`[]= (write)`      - func S method(a T1, val T2)
			`<--> (read)`      - func S method() (T1, T2)
			`<-->= (write)`    - func S method(a, b T)
			`* (deref read)`   - func S method() T
			`*= (deref write)` - func S method(val T)
		```
		* Host API overloadable operators:
			* binary: `+, -, *, /, %, &, ^, |, &^, <<, >>, >>>, **, &&, ||, ==, <=, >=, !=, =, .` [=]
			* unary:  `+, -, *, &, <-, ->, <-var->, [], !, ^, [::], {}` [=]
		* script overloadable built-ins:
		```
			`new`              - func new(T) *T
			`delete`           - func delete(a T, ...any)
			`append`           - func append(a T, b ...T) T
			`copy`             - func copy(dst, src T) uint
			`len`              - func len(a T) uint
			`cap`              - func cap(a T) uint
			`make`             - func make(T, sizes ...uint) T
			`range`            - func range(a T1) (T2, T3)
		```

* Light, Expressive Syntax.
	* added ternary operator, cannot be nested!

* Efficient garbage collection that has little-to-no impact on app performance.
	* Incremental/Concurrent Mark & Sweep with generations + lazy sweeping.
	* When you have objects of same size in block then walking block is easy.

* Hot-loadable.
* Inter-script communications to handle complex scripts.

* Minimal Standard Library:
	* string manipulation
	* math manipulation - (if not already provided by the host app)
	* (file) IO - (add logging)
	* sorting - (just have merge-sorts?)
	* time

* Register/Stack-based virtual runtime.

* Built-in dynamic array & hash map containers.

* JIT compilation (or code generation) for stronger performance (?).

* Default Function Arguments.

* Default Mutable String types (base on char-vector) for better performance with string manipulation library.

* Enum/Const values with special incrementing syntax like: (Q? iota value with control of how it increments?)
	* enum ( <<= 1 ) { values };

* No background/hidden state execution that wasn't done by the script developer.


- Allows designers to build complex gameplay elements, define complex ai behaviors, create gameflow with minimal work from the software engineers.
- Handles memory allocation/destruction behind the scenes
- Does not crash the game when an error occurs
- Handles multithreading and/or events
- Does not allow designers to shoot themselves in the foot
- Has a simple and clean syntax
- Allows software engineers to expose their APIs to it easily
- Can be reloaded on the fly
