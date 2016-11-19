# 2603fa6a-ed09-404f-aa21-e8711eb7fb4d

## The Rule

Do not use an enumeration type defined in another assembly as the declared type
of an input, except in the following cases:

- The precondition of the input restricts valid values to a specific subset of
  the enumeration type.

## Discussion

# 0def3d8e-5043-4500-8e2d-1206186de64c

## The Rule

Never cast the underlying integral type of an enumeration to an enumeration
type.

## Discussion

Casting from the underlying integral type of an enumeration to an enumeration
type can lead an invalid enumeration value, which is a design by contract
violation. The contract for any type (including an enumeration type) is that
all instances of the type are valid members of the type. Therefore, the mere
existence of an invalid enumeration value is a design by contract violation.
Normally a cast that results in this kind of "type contract" violation causes a
compile or run time error, but for enumerations the validity of the cast is not
checked at compile or run time.

You may cast an enumeration type to the underlying integral type (the inverse
of what this rule prohibits) since the the resulting integral value is
guaranteed to be a valid member of the underlying integral type.

If you are casting an integral value that is known at compile time (a constant)
to an enumeration type, use the named enumeration member instead. This ensures
that if the enumeration member is assigned a new value in the future, your
assembly will function as intended when it is recompiled. And if the
enumeration member is removed in the future, your assembly will not compile
until you select a new enumeration member that exists in the current
compilation. Therefore, using a named enumeration member instead of casting
from an integral constant ensures that you will never produce an invalid
enumeration value.

If you are casting from an integral value that is not known at compile time,
then you cannot (in the general case) ensure that the result of the cast is a
valid enumeration value for every compilation of the assembly. While direct
casting to the enumeration type is forbidden by the rule, you can use a
`switch` statement on the integral value with `case` labels for each named
enumeration member you want to test for. Notice that the `case` labels cast the
enumeration type to the underlying integral type, and this is not forbidden by
the rule. You may use the `switch` method to find an enumeration member whose
value is equal to a particular integer, but you must always handle the case
where no matching enumeration member is found.

# 375a94f0-984d-4807-9fa3-7acf1005d492

## The Rule

Never handle invalid enumeration values.

## Discussion

When you use an enumeration type as the declared type of an input, you create
an explicit precondition that the value passed from calling code is a valid
member of the enumeration type. Valid members of an enumeration type are those
defined in the enumeration at the time your assembly is compiled. Since an
invalid enumeration value is a precondition violation, it must not be handled
(1a602c70-f177-47ad-a023-87fec7ec5146).

If your code cannot rely on callers to ensure this precondition, then there is
no added benefit from using the enumeration type as the declared type of the
input, and you must instead use the enumeration's underlying integral type as
the declared type of the input.

You may flag invalid enumeration values in debug builds (e.g. by throwing an
exception). This is an instance of precondition checking.

## See Also

- 2603fa6a-ed09-404f-aa21-e8711eb7fb4d

# 1a602c70-f177-47ad-a023-87fec7ec5146

## The Rule

Never handle a precondition violation.
