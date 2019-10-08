# Compat Package for Julia

[![Build Status](https://travis-ci.org/JuliaLang/Compat.jl.svg?branch=master)](https://travis-ci.org/JuliaLang/Compat.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/github/JuliaLang/Compat.jl?branch=master)](https://ci.appveyor.com/project/quinnj/compat-jl/branch/master)

The **Compat** package is designed to ease interoperability between
older and newer versions of the [Julia
language](http://julialang.org/).  In particular, in cases where it is
impossible to write code that works with both the latest Julia
`master` branch and older Julia versions, or impossible to write code
that doesn't generate a deprecation warning in some Julia version, the
Compat package provides a macro that lets you use the *latest syntax*
in a backwards-compatible way.

This is primarily intended for use by other [Julia
packages](http://docs.julialang.org/en/latest/manual/packages/), where
it is important to maintain cross-version compatibility.

## Usage

To use Compat in your Julia package, add a line `Compat` to the
`REQUIRE` file in your package directory.  Then, in your package,
shortly after the `module` statement include lines like these:

```julia
using Compat
import Compat.String
```

and then as needed add

```julia
@compat ...compat syntax...
```

wherever you want to use syntax that differs in the latest Julia
`master` (the development version of Julia). The `compat syntax` is usually
the syntax on Julia `master`. However, in a few cases where this is not possible,
a slightly different syntax might be used.
Please check the list below for the specific syntax you need.

## Supported syntax

Currently, the `@compat` macro supports the following syntaxes:

## Module Aliases

## New functions, macros, and methods

* `only(x)` returns the one-and-only element of a collection `x`. ([#33129])

* `mod` now accepts a unit range as the second argument ([#32628]).

* `eachrow`, `eachcol`, and `eachslice` to iterate over first, second, or given dimension
  of an array ([#29749]).

* `isnothing` for testing if a variable is equal to `nothing` ([#29674]).

* `range` supporting `stop` as positional argument ([#28708]).

* `hasproperty` and `hasfield` ([#28850]).
  `hasproperty` is defined only for Julia 0.7 or later.

* `merge` methods with one and `n` `NamedTuple`s ([#29259]).

## Renaming

## New macros

## Other changes

* On versions of Julia that do not contain a Base.Threads module, Compat defines a Threads module containing a no-op `@threads` macro.

## New types

## Developer tips

One of the most important rules for `Compat.jl` is to avoid breaking user code
whenever possible, especially on a released version.

Although the syntax used in the most recent Julia version
is the preferred compat syntax, there are cases where this shouldn't be used.
Examples include when the new syntax already has a different meaning
on previous versions of Julia, or when functions are removed from `Base`
Julia and the alternative cannot be easily implemented on previous versions.
In such cases, possible solutions are forcing the new feature to be used with
qualified name in `Compat.jl` (e.g. use `Compat.<name>`) or
reimplementing the old features on a later Julia version.

If you're adding additional compatibility code to this package, the [`contrib/commit-name.sh`](https://github.com/JuliaLang/julia/blob/master/contrib/commit-name.sh) script in the base Julia repository is useful for extracting the version number from a git commit SHA. For example, from the git repository of `julia`, run something like this:

```sh
bash $ contrib/commit-name.sh a378b60fe483130d0d30206deb8ba662e93944da
0.5.0-dev+2023
```

This prints a version number corresponding to the specified commit of the form
`X.Y.Z-aaa+NNNN`, and you can then test whether Julia
is at least this version by `VERSION >= v"X.Y.Z-aaa+NNNN"`.

### Tagging the correct minimum version of Compat

One of the most frequent problems package developers encounter is finding the right
version of `Compat` to add to their REQUIRE. This is meant to be a guide on how to
specify the right lower bound.

* Find the appropriate fix needed for your package from the `Compat` README. Every
function or feature added to `Compat` is documented in its README, so you are
guaranteed to find it.

* Navigate to the [blame page of the README](https://github.com/JuliaLang/Compat.jl/blame/master/README.md)
by clicking on the README file on GitHub, and then clicking on the `blame` button
which can be found in the top-right corner.

* Now find your fix, and then find the corresponding commit ID of that fix on the
left-hand side. Click on the commit ID. This navigates to a page which recorded
that particular commit.

* On the top pane, you should find the list of the tagged versions of Compat that
includes this fix. Find the minimum version from there.

* Now specify the correct minimum version for Compat in your REQUIRE file by
`Compat <version>`

[#20005]: https://github.com/JuliaLang/julia/issues/20005
[#20974]: https://github.com/JuliaLang/julia/issues/20974
[#21197]: https://github.com/JuliaLang/julia/issues/21197
[#21709]: https://github.com/JuliaLang/julia/issues/21709
[#22064]: https://github.com/JuliaLang/julia/issues/22064
[#22182]: https://github.com/JuliaLang/julia/issues/22182
[#22350]: https://github.com/JuliaLang/julia/issues/22350
[#22435]: https://github.com/JuliaLang/julia/issues/22435
[#22475]: https://github.com/JuliaLang/julia/issues/22475
[#22512]: https://github.com/JuliaLang/julia/issues/22512
[#22629]: https://github.com/JuliaLang/julia/issues/22629
[#22646]: https://github.com/JuliaLang/julia/issues/22646
[#22666]: https://github.com/JuliaLang/julia/issues/22666
[#22751]: https://github.com/JuliaLang/julia/issues/22751
[#22761]: https://github.com/JuliaLang/julia/issues/22761
[#22864]: https://github.com/JuliaLang/julia/issues/22864
[#22907]: https://github.com/JuliaLang/julia/issues/22907
[#23051]: https://github.com/JuliaLang/julia/issues/23051
[#23235]: https://github.com/JuliaLang/julia/issues/23235
[#23271]: https://github.com/JuliaLang/julia/issues/23271
[#23412]: https://github.com/JuliaLang/julia/issues/23412
[#23427]: https://github.com/JuliaLang/julia/issues/23427
[#23570]: https://github.com/JuliaLang/julia/issues/23570
[#23642]: https://github.com/JuliaLang/julia/issues/23642
[#23666]: https://github.com/JuliaLang/julia/issues/23666
[#23667]: https://github.com/JuliaLang/julia/issues/23667
[#23757]: https://github.com/JuliaLang/julia/issues/23757
[#23805]: https://github.com/JuliaLang/julia/issues/23805
[#23931]: https://github.com/JuliaLang/julia/issues/23931
[#24047]: https://github.com/JuliaLang/julia/issues/24047
[#24182]: https://github.com/JuliaLang/julia/issues/24182
[#24282]: https://github.com/JuliaLang/julia/issues/24282
[#24361]: https://github.com/JuliaLang/julia/issues/24361
[#24372]: https://github.com/JuliaLang/julia/issues/24372
[#24414]: https://github.com/JuliaLang/julia/issues/24414
[#24443]: https://github.com/JuliaLang/julia/issues/24443
[#24459]: https://github.com/JuliaLang/julia/issues/24459
[#24490]: https://github.com/JuliaLang/julia/issues/24490
[#24605]: https://github.com/JuliaLang/julia/issues/24605
[#24647]: https://github.com/JuliaLang/julia/issues/24647
[#24652]: https://github.com/JuliaLang/julia/issues/24652
[#24657]: https://github.com/JuliaLang/julia/issues/24657
[#24673]: https://github.com/JuliaLang/julia/issues/24673
[#24785]: https://github.com/JuliaLang/julia/issues/24785
[#24808]: https://github.com/JuliaLang/julia/issues/24808
[#24831]: https://github.com/JuliaLang/julia/issues/24831
[#24839]: https://github.com/JuliaLang/julia/issues/24839
[#24874]: https://github.com/JuliaLang/julia/issues/24874
[#24999]: https://github.com/JuliaLang/julia/issues/24999
[#25012]: https://github.com/JuliaLang/julia/issues/25012
[#25021]: https://github.com/JuliaLang/julia/issues/25021
[#25056]: https://github.com/JuliaLang/julia/issues/25056
[#25057]: https://github.com/JuliaLang/julia/issues/25057
[#25100]: https://github.com/JuliaLang/julia/issues/25100
[#25102]: https://github.com/JuliaLang/julia/issues/25102
[#25113]: https://github.com/JuliaLang/julia/issues/25113
[#25162]: https://github.com/JuliaLang/julia/issues/25162
[#25165]: https://github.com/JuliaLang/julia/issues/25165
[#25168]: https://github.com/JuliaLang/julia/issues/25168
[#25227]: https://github.com/JuliaLang/julia/issues/25227
[#25241]: https://github.com/JuliaLang/julia/issues/25241
[#25249]: https://github.com/JuliaLang/julia/issues/25249
[#25402]: https://github.com/JuliaLang/julia/issues/25402
[#25458]: https://github.com/JuliaLang/julia/issues/25458
[#25459]: https://github.com/JuliaLang/julia/issues/25459
[#25479]: https://github.com/JuliaLang/julia/issues/25479
[#25496]: https://github.com/JuliaLang/julia/issues/25496
[#25522]: https://github.com/JuliaLang/julia/issues/25522
[#25544]: https://github.com/JuliaLang/julia/issues/25544
[#25545]: https://github.com/JuliaLang/julia/issues/25545
[#25571]: https://github.com/JuliaLang/julia/issues/25571
[#25615]: https://github.com/JuliaLang/julia/issues/25615
[#25616]: https://github.com/JuliaLang/julia/issues/25616
[#25622]: https://github.com/JuliaLang/julia/issues/25622
[#25628]: https://github.com/JuliaLang/julia/issues/25628
[#25629]: https://github.com/JuliaLang/julia/issues/25629
[#25634]: https://github.com/JuliaLang/julia/issues/25634
[#25646]: https://github.com/JuliaLang/julia/issues/25646
[#25647]: https://github.com/JuliaLang/julia/issues/25647
[#25654]: https://github.com/JuliaLang/julia/issues/25654
[#25662]: https://github.com/JuliaLang/julia/issues/25662
[#25701]: https://github.com/JuliaLang/julia/issues/25701
[#25705]: https://github.com/JuliaLang/julia/issues/25705
[#25706]: https://github.com/JuliaLang/julia/issues/25706
[#25738]: https://github.com/JuliaLang/julia/issues/25738
[#25780]: https://github.com/JuliaLang/julia/issues/25780
[#25812]: https://github.com/JuliaLang/julia/issues/25812
[#25819]: https://github.com/JuliaLang/julia/issues/25819
[#25830]: https://github.com/JuliaLang/julia/issues/25830
[#25873]: https://github.com/JuliaLang/julia/issues/25873
[#25896]: https://github.com/JuliaLang/julia/issues/25896
[#25935]: https://github.com/JuliaLang/julia/issues/25935
[#25940]: https://github.com/JuliaLang/julia/issues/25940
[#25959]: https://github.com/JuliaLang/julia/issues/25959
[#25989]: https://github.com/JuliaLang/julia/issues/25989
[#25990]: https://github.com/JuliaLang/julia/issues/25990
[#25998]: https://github.com/JuliaLang/julia/issues/25998
[#26009]: https://github.com/JuliaLang/julia/issues/26009
[#26039]: https://github.com/JuliaLang/julia/issues/26039
[#26069]: https://github.com/JuliaLang/julia/issues/26069
[#26089]: https://github.com/JuliaLang/julia/issues/26089
[#26149]: https://github.com/JuliaLang/julia/issues/26149
[#26156]: https://github.com/JuliaLang/julia/issues/26156
[#26283]: https://github.com/JuliaLang/julia/issues/26283
[#26316]: https://github.com/JuliaLang/julia/issues/26316
[#26365]: https://github.com/JuliaLang/julia/issues/26365
[#26369]: https://github.com/JuliaLang/julia/issues/26369
[#26436]: https://github.com/JuliaLang/julia/issues/26436
[#26442]: https://github.com/JuliaLang/julia/issues/26442
[#26486]: https://github.com/JuliaLang/julia/issues/26486
[#26559]: https://github.com/JuliaLang/julia/issues/26559
[#26634]: https://github.com/JuliaLang/julia/issues/26634
[#26660]: https://github.com/JuliaLang/julia/issues/26660
[#26670]: https://github.com/JuliaLang/julia/issues/26670
[#26850]: https://github.com/JuliaLang/julia/issues/26850
[#27077]: https://github.com/JuliaLang/julia/issues/27077
[#27163]: https://github.com/JuliaLang/julia/issues/27163
[#27253]: https://github.com/JuliaLang/julia/issues/27253
[#27258]: https://github.com/JuliaLang/julia/issues/27258
[#27298]: https://github.com/JuliaLang/julia/issues/27298
[#27401]: https://github.com/JuliaLang/julia/issues/27401
[#27711]: https://github.com/JuliaLang/julia/issues/27711
[#27828]: https://github.com/JuliaLang/julia/issues/27828
[#27834]: https://github.com/JuliaLang/julia/issues/27834
[#28295]: https://github.com/JuliaLang/julia/issues/28295
[#28302]: https://github.com/JuliaLang/julia/issues/28302
[#28303]: https://github.com/JuliaLang/julia/issues/28303
[#28708]: https://github.com/JuliaLang/julia/issues/28708
[#28850]: https://github.com/JuliaLang/julia/issues/28850
[#29259]: https://github.com/JuliaLang/julia/issues/29259
[#29674]: https://github.com/JuliaLang/julia/issues/29674
[#29749]: https://github.com/JuliaLang/julia/issues/29749
[#33129]: https://github.com/JuliaLang/julia/issues/33129
[#32628]: https://github.com/JuliaLang/julia/issues/32628
