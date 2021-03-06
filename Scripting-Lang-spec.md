# Top grammar
```ebnf
SourceFile     = ImportSpec ";" { ImportDecl ";" } { TopLevelDecl ";" } .
ImportSpec     = "import" <id> .
```

# Import grammar
```ebnf
ImportSpec     = [ "." | <id> ] <string> .
ImportDecl     = "import" ( ImportSpec | "(" { ImportSpec ";" } ")" ) .
```
```go
import   "lib/math" ==> math.Sin
import m "lib/math" ==> m.Sin
import . "lib/math" ==> Sin
```

# Decl grammar
```ebnf
TopLevelDecl   = Declaration | FunctionDecl | MethodDecl | OpOverloadDecl .

Declaration    = ConstDecl | TypeDecl | VarDecl .

ConstDecl      = "const" ( ConstSpec | "(" { ConstSpec ";" } ")" ) .
ConstSpec      = IdentifierList [ [ Type ] "=" ExpressionList ] .

IdentifierList = identifier { "," identifier } .
ExpressionList = Expression { "," Expression } .

TypeDecl       = "type" ( TypeSpec | "(" { TypeSpec ";" } ")" ) .
TypeSpec       = AliasDecl | TypeDef .

GenericSpec    = "[" GenericList "]" .
GenericList    = GenericDecl { "," GenericDecl } .
GenericDecl    = IdentifierList Type .

AliasDecl      = identifier [ GenericSpec ] "=" Type .
TypeDef        = identifier [ GenericSpec ] Type .

VarDecl        = "var" ( VarSpec | "(" { VarSpec ";" } ")" ) .
VarSpec        = IdentifierList ( Type [ "=" ExpressionList ] | "=" ExpressionList ) .
```

# Func Decl Grammar
```ebnf
FunctionDecl   = "func" FuncName [ GenericSpec ] Signature [ Block ] .

## these are the only operators allowed to be overloaded from script level.
## Anything else should be implemented via Host app API for full control of operator overloading.
OpOverloadDecl = "func" "(" identifier Type ")" AllowOps Signature [ Block ] .
AllowOps       = ("[" "]" | "<-" | "->" | "<-->" | "*" | "." ) [ "=" ] .

MethodDecl     = "func" "(" identifier Type ")" MethodName [ GenericSpec ] Signature [ Block ] .

FunctionType   = "func" Signature .
Signature      = Parameters [ Result ] .
Result         = Parameters | Type .
Parameters     = "(" [ ParameterList [ "," ] ] ")" .
ParameterList  = ParameterDecl { "," ParameterDecl } .
ParameterDecl  = [ IdentifierList ] [ "..." ] Type .
```

# Block grammar
```ebnf
Block          = "{" StatementList "}" .
StatementList  = { Statement ";" } .
Statement      = Declaration | LabeledStmt | SimpleStmt | ReturnStmt | BreakStmt | ContinueStmt | GotoStmt | FallthruStmt | Block | IfStmt | SwitchStmt | ForStmt | DeferStmt .

SimpleStmt     = EmptyStmt | ExpressionStmt | IncDecStmt | Assignment | ShortVarDecl .


LabeledStmt    = identifier ":" Statement .
ReturnStmt     = "return" [ ExpressionList ] .
BreakStmt      = "break" [ Label ] .
ContinueStmt   = "continue" [ Label ] .
GotoStmt       = "goto" Label .
FallthruStmt   = "fallthru" .
IfStmt         = "if" [ SimpleStmt ";" ] Expression Block [ "else" ( IfStmt | Block ) ] .
ShortVarDecl   = IdentifierList ":=" ExpressionList .

SwitchStmt     = ExprSwitchStmt .
ExprSwitchStmt = "switch" [ SimpleStmt ";" ] [ (Expression | PrimaryExpr "." "(" "type" ")") ] "{" { ExprCaseClause } "}" .
ExprCaseClause = ExprSwitchCase ":" StatementList .
ExprSwitchCase = "case" (ExpressionList | TypeList) | "default" .
TypeList       = Type { "," Type } .

ForStmt        = "for" [ Condition | ForClause | RangeClause ] Block .
RangeClause    = [ ExpressionList "=" | IdentifierList ":=" ] "range" Expression .
Condition      = Expression .
ForClause      = [ InitStmt ] ";" [ Condition ] ";" [ PostStmt ] .
InitStmt       = SimpleStmt .
PostStmt       = SimpleStmt .

DeferStmt      = "defer" Expression .

ExpressionStmt = Expression .

IncDecStmt     = Expression ( "++" | "--" ) .

Assignment     = ExpressionList assign_op ExpressionList .
assign_op      = [ add_op | mul_op | pow_op ] "=" .
```

# Expression grammar
```ebnf
Expression     = MainExpr | TernaryExpr.
MainExpr       = UnaryExpr | MainExpr binary_op MainExpr .
TernaryExpr    = "if" "(" Condition ")" MainExpr "else" MainExpr .
BinaryExpr     = MainExpr binary_op MainExpr .
UnaryExpr      = PrimaryExpr | unary_op UnaryExpr .

binary_op      = "||" | "&&" | rel_op | add_op | mul_op | pow_op .
rel_op         = "==" | "!=" | "<" | "<=" | ">" | ">=" .
add_op         = "+" | "-" | "|" | "^" .
mul_op         = "*" | "/" | "%" | "<<" | ">>" | ">>>" | "&" | "&^" .
pow_op         = "**" .
unary_op       = "+" | "-" | "!" | "^" | "*" | "&" | "<-" | "~" .


PrimaryExpr    = Operand | Conversion | MethodExpr | PrimaryExpr Selector | PrimaryExpr Index | PrimaryExpr MultiIndex | PrimaryExpr Slice | PrimaryExpr Arguments | PrimaryExpr RightNode | PrimaryExpr GenericExpr .

Selector       = "." identifier .
Index          = "[" Expression "]" .
MultiIndex     = "[" ExpressionList "]" .
GenericExpr    = "[" TypeList "]" .
Slice          = "[" [ Expression ] ":" [ Expression ] "]" |
                 "[" [ Expression ] ":" Expression ":" Expression "]" .
TypeAssertion  = "." "(" Type ")" .
Arguments      = "(" [ ( ExpressionList | Type [ "," ExpressionList ] ) [ "..." ] [ "," ] ] ")" .
RightNode      = "->" .

MethodExpr     = ReceiverType "." MethodName .
ReceiverType   = Type .

Conversion     = Type "(" Expression [ "," ] ")" .

Operand        = Literal | OperandName | "(" Expression ")" .
Literal        = BasicLit | CompositeLit | FunctionLit .
BasicLit       = int_lit | float_lit | rune_lit | string_lit .
OperandName    = identifier | QualifiedIdent .
```

# Type grammar
```ebnf
Type           = TypeName | TypeLit | "(" Type ")" .
TypeName       = identifier | QualifiedIdent .
TypeLit        = ArrayType | StructType | PointerType | FunctionType | InterfaceType | SliceType | MapType .

ArrayType      = "[" Expression "]" Type .
SliceType      = "[" "]" Type .
MapType        = "map" "[" KeyType "]" ValType .
KeyType        = Type .
ValType        = Type .

StructType     = "struct" "{" { FieldDecl ";" } "}" .
FieldDecl      = (IdentifierList Type | EmbeddedField) .
EmbeddedField  = [ "&" ] TypeName .
PointerType    = "&" TypeName .

InterfaceType  = "interface" "{" { ( MethodSpec | TypeName ) ";" } "}" .
MethodSpec     = identifier Signature .
```
