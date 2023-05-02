# structs
--
    import "."

Package structs contains various utilities functions to work with structs.

## Usage

```go
var (
	// DefaultTagName is the default tag name for struct fields which provides
	// a more granular to tweak certain structs. Lookup the necessary functions
	// for more info.
	DefaultTagName = "structs" // struct's field default tag name
)
```

#### func  FillMap

```go
func FillMap(s interface{}, out map[string]interface{})
```
FillMap is the same as Map. Instead of returning the output, it fills the given
map.

#### func  HasZero

```go
func HasZero(s interface{}) bool
```
HasZero returns true if any field is equal to a zero value. For more info refer
to Struct types HasZero() method. It panics if s's kind is not struct.

#### func  IsStruct

```go
func IsStruct(s interface{}) bool
```
IsStruct returns true if the given variable is a struct or a pointer to struct.

#### func  IsZero

```go
func IsZero(s interface{}) bool
```
IsZero returns true if all fields is equal to a zero value. For more info refer
to Struct types IsZero() method. It panics if s's kind is not struct.

#### func  Map

```go
func Map(s interface{}) map[string]interface{}
```
Map converts the given struct to a map[string]interface{}. For more info refer
to Struct types Map() method. It panics if s's kind is not struct.

#### func  Name

```go
func Name(s interface{}) string
```
Name returns the structs's type name within its package. It returns an empty
string for unnamed types. It panics if s's kind is not struct.

#### func  Names

```go
func Names(s interface{}) []string
```
Names returns a slice of field names. For more info refer to Struct types
Names() method. It panics if s's kind is not struct.

#### func  Values

```go
func Values(s interface{}) []interface{}
```
Values converts the given struct to a []interface{}. For more info refer to
Struct types Values() method. It panics if s's kind is not struct.

#### type Field

```go
type Field struct {
}
```

Field represents a single struct field that encapsulates high level functions
around the field.

#### func  Fields

```go
func Fields(s interface{}) []*Field
```
Fields returns a slice of *Field. For more info refer to Struct types Fields()
method. It panics if s's kind is not struct.

#### func (*Field) Field

```go
func (f *Field) Field(name string) *Field
```
Field returns the field from a nested struct. It panics if the nested struct is
not exported or if the field was not found.

#### func (*Field) FieldOk

```go
func (f *Field) FieldOk(name string) (*Field, bool)
```
FieldOk returns the field from a nested struct. The boolean returns whether the
field was found (true) or not (false).

#### func (*Field) Fields

```go
func (f *Field) Fields() []*Field
```
Fields returns a slice of Fields. This is particular handy to get the fields of
a nested struct . A struct tag with the content of "-" ignores the checking of
that particular field. Example:

    // Field is ignored by this package.
    Field *http.Request `structs:"-"`

It panics if field is not exported or if field's kind is not struct

#### func (*Field) IsEmbedded

```go
func (f *Field) IsEmbedded() bool
```
IsEmbedded returns true if the given field is an anonymous field (embedded)

#### func (*Field) IsExported

```go
func (f *Field) IsExported() bool
```
IsExported returns true if the given field is exported.

#### func (*Field) IsZero

```go
func (f *Field) IsZero() bool
```
IsZero returns true if the given field is not initialized (has a zero value). It
panics if the field is not exported.

#### func (*Field) Kind

```go
func (f *Field) Kind() reflect.Kind
```
Kind returns the fields kind, such as "string", "map", "bool", etc ..

#### func (*Field) Name

```go
func (f *Field) Name() string
```
Name returns the name of the given field

#### func (*Field) Set

```go
func (f *Field) Set(val interface{}) error
```
Set sets the field to given value v. It returns an error if the field is not
settable (not addressable or not exported) or if the given value's type doesn't
match the fields type.

#### func (*Field) Tag

```go
func (f *Field) Tag(key string) string
```
Tag returns the value associated with key in the tag string. If there is no such
key in the tag, Tag returns the empty string.

#### func (*Field) Value

```go
func (f *Field) Value() interface{}
```
Value returns the underlying value of the field. It panics if the field is not
exported.

#### func (*Field) Zero

```go
func (f *Field) Zero() error
```
Zero sets the field to its zero value. It returns an error if the field is not
settable (not addressable or not exported).

#### type Struct

```go
type Struct struct {
	TagName string
}
```

Struct encapsulates a struct type to provide several high level functions around
the struct.

#### func  New

```go
func New(s interface{}) *Struct
```
New returns a new *Struct with the struct s. It panics if the s's kind is not
struct.

#### func (*Struct) Field

```go
func (s *Struct) Field(name string) *Field
```
Field returns a new Field struct that provides several high level functions
around a single struct field entity. It panics if the field is not found.

#### func (*Struct) FieldOk

```go
func (s *Struct) FieldOk(name string) (*Field, bool)
```
FieldOk returns a new Field struct that provides several high level functions
around a single struct field entity. The boolean returns true if the field was
found.

#### func (*Struct) Fields

```go
func (s *Struct) Fields() []*Field
```
Fields returns a slice of Fields. A struct tag with the content of "-" ignores
the checking of that particular field. Example:

    // Field is ignored by this package.
    Field bool `structs:"-"`

It panics if s's kind is not struct.

#### func (*Struct) FillMap

```go
func (s *Struct) FillMap(out map[string]interface{})
```
FillMap is the same as Map. Instead of returning the output, it fills the given
map.

#### func (*Struct) HasZero

```go
func (s *Struct) HasZero() bool
```
HasZero returns true if a field in a struct is not initialized (zero value). A
struct tag with the content of "-" ignores the checking of that particular
field. Example:

    // Field is ignored by this package.
    Field bool `structs:"-"`

A value with the option of "omitnested" stops iterating further if the type is a
struct. Example:

    // Field is not processed further by this package.
    Field time.Time     `structs:"myName,omitnested"`
    Field *http.Request `structs:",omitnested"`

Note that only exported fields of a struct can be accessed, non exported fields
will be neglected. It panics if s's kind is not struct.

#### func (*Struct) IsZero

```go
func (s *Struct) IsZero() bool
```
IsZero returns true if all fields in a struct is a zero value (not initialized)
A struct tag with the content of "-" ignores the checking of that particular
field. Example:

    // Field is ignored by this package.
    Field bool `structs:"-"`

A value with the option of "omitnested" stops iterating further if the type is a
struct. Example:

    // Field is not processed further by this package.
    Field time.Time     `structs:"myName,omitnested"`
    Field *http.Request `structs:",omitnested"`

Note that only exported fields of a struct can be accessed, non exported fields
will be neglected. It panics if s's kind is not struct.

#### func (*Struct) Map

```go
func (s *Struct) Map() map[string]interface{}
```
Map converts the given struct to a map[string]interface{}, where the keys of the
map are the field names and the values of the map the associated values of the
fields. The default key string is the struct field name but can be changed in
the struct field's tag value. The "structs" key in the struct's field tag value
is the key name. Example:

    // Field appears in map as key "myName".
    Name string `structs:"myName"`

A tag value with the content of "-" ignores that particular field. Example:

    // Field is ignored by this package.
    Field bool `structs:"-"`

A tag value with the content of "string" uses the stringer to get the value.
Example:

    // The value will be output of Animal's String() func.
    // Map will panic if Animal does not implement String().
    Field *Animal `structs:"field,string"`

A tag value with the option of "flatten" used in a struct field is to flatten
its fields in the output map. Example:

    // The FieldStruct's fields will be flattened into the output map.
    FieldStruct time.Time `structs:",flatten"`

A tag value with the option of "omitnested" stops iterating further if the type
is a struct. Example:

    // Field is not processed further by this package.
    Field time.Time     `structs:"myName,omitnested"`
    Field *http.Request `structs:",omitnested"`

A tag value with the option of "omitempty" ignores that particular field if the
field value is empty. Example:

    // Field appears in map as key "myName", but the field is
    // skipped if empty.
    Field string `structs:"myName,omitempty"`

    // Field appears in map as key "Field" (the default), but
    // the field is skipped if empty.
    Field string `structs:",omitempty"`

Note that only exported fields of a struct can be accessed, non exported fields
will be neglected.

#### func (*Struct) Name

```go
func (s *Struct) Name() string
```
Name returns the structs's type name within its package. For more info refer to
Name() function.

#### func (*Struct) Names

```go
func (s *Struct) Names() []string
```
Names returns a slice of field names. A struct tag with the content of "-"
ignores the checking of that particular field. Example:

    // Field is ignored by this package.
    Field bool `structs:"-"`

It panics if s's kind is not struct.

#### func (*Struct) Values

```go
func (s *Struct) Values() []interface{}
```
Values converts the given s struct's field values to a []interface{}. A struct
tag with the content of "-" ignores the that particular field. Example:

    // Field is ignored by this package.
    Field int `structs:"-"`

A value with the option of "omitnested" stops iterating further if the type is a
struct. Example:

    // Fields is not processed further by this package.
    Field time.Time     `structs:",omitnested"`
    Field *http.Request `structs:",omitnested"`

A tag value with the option of "omitempty" ignores that particular field and is
not added to the values if the field value is empty. Example:

    // Field is skipped if empty
    Field string `structs:",omitempty"`

Note that only exported fields of a struct can be accessed, non exported fields
will be neglected.
