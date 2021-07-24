/// Type classes:
Integers => int, uint, int64, int32, int16, int8, bool, uint64, uint32, uint16, uint8, byte=uint8, rune=int32.
	* smaller ints implicitly cast to larger ints.
	* larger ints require explicit casts to smaller ints.
	* casting larger signed to smaller signed will keep signedness.
	* casting smaller signed to larger signed will sign extend.
	* casting smaller uint to larger signed int is implicit.
	* casting smaller int to larger uint requires explicit cast. Warning otherwise.
	* mixing int     + uint   = uint.   Warning without explicit cast.
	* mixing smaller + larger = larger. Warning without explicit cast.
	
Floats => float32, float64, floatmax.
	* mixing smaller + larger = larger implicitly.
	* casting to and from int and float32/float64 is implicit.
		* casting to and from uint requires explicit cast, warning otherwise.

Aggregates => struct{}, interface{}, [n]type, []type, map[type, type], set[type], string.
	* strings are implicitly interchangeable with [](u)int8/byte and [](u)int32/rune.
	* [n]type is implicitly casted to []type. Vice versa is illegal and requires copying instead.
	* set[type] can be implicitly casted to []type but not vice versa.
	* arrays of the same type but different sizes can be assigned to each other but copied only by the shortest of the two.
		* When done during declaration, if smaller, the remaining values will be zero-initialized.
	* struct{type}, where all fields are of a single type, is interchangeable with []type.
		* if fixed-size, the [n]type array must be equal-or-larger than the member count of the struct.
* Typedef'd names will take on the type class they're based on.
	
Specials => func(params)type, *type.
	* pointers are distant-cousins of Integer type class.
		* pointer values can be implicitly casted to (u)int only.
		* smaller ints require explicit cast.
		* vice versa is illegal.
	* pointers cannot refer to variables of a lower scope.
	* each specific function variation is its own type and illegal to cast.
