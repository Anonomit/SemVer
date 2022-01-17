# SemVer.lua

Semantic versioning for Lua.

See http://semver.org/ for details about semantic versioning.


# Documentation

``` lua
local v = LibStub'SemVer'

-- two ways of creating it: with separate parameters, or with one string
v1 = v(1,0,0)
v2_5_1 = v("2.5.1")

-- When using one string one can skip the parenthesis:
v2_5_1 = v"2.5.1" -- valid in Lua

-- major, minor and patch attributes
v2_5_1.major -- 2
v2_5_1.minor -- 5
v2_5_1.patch -- 1

-- prereleases:
a = v(1,0,0,"alpha")
a.prerelease -- "alpha"
b = v("1.0.0-beta")
b.prerelease -- "beta"

-- builds
c = v(1,0,0,nil,"build-1")
c.build -- "build-1"

d = v("0.9.5+no.extensions.22")
d.build -- "no.extensions.22"

-- comparison & sorting
v"1.2.3" == v(1,2,3)        -- true
v"1.2.3" < v(4,5,6)         -- true
v"1.2.3-alpha" < v"1.2.3"   -- true
v"1.2.3" < v"1.2.3+build.1" -- false, builds are ignored when comparing versions in SemVer
-- (see the "notes" section for more informaion about version comparison)

-- compatibility operator: ^
-- a ^ b returns true if a and b are compatible
-- intended to be used for version comparison between users
v"2.0.1" ^ v"2.5.1" -- true  - it's safe for versions 2.0.1 and 2.5.1 to communicate
v"2.5.1" ^ v"2.0.1" -- true  - it's safe for versions 2.0.1 and 2.5.1 to communicate
v"1.0.0" ^ v"2.0.0" -- false - 2.0.0 is not supposed to be backwards-compatible
v"0.0.1" ^ v"0.5.1" -- false - Prerelease versions must be identical to be considered compatible

-- getting newer versions
v(1,0,0):NextPatch() -- v1.0.1
v(1,2,3):NextMinor() -- v1.3.0 . Notice the patch resets to 0
v(1,2,3):NextMajor() -- v2.0.0 . Minor and patch are reset to 0

```


# Installation

Install as you would with any other addon.

For developers:
You can include SemVer as a library in whatever addon you want (for example in a libs/ folder). Make sure the toc file includes SemVer.xml or SemVer.lua. LibStub is a required library, so be sure to include that too (if it isn't already).


Write this in any lua file where you want to use it:

``` lua
local v = LibStub"SemVer"
```

Using `v` allows for the nice string syntax: `v"1.2.3-alpha"`.

You can use SemVer instead:

``` lua
local SemVer = LibStub"SemVer"
```

Please make sure that you read the license, too.


# Notes about version comparison

Version comparison is done according to the SemVer 2.0.0 specs:

Major, minor, and patch versions are always compared numerically.

Pre-release precedence MUST be determined by comparing each dot-separated identifier as follows:

* Identifiers consisting of only digits are compared numerically
* Identifiers with letters or dashes are compared lexically in ASCII sort order.
* Numeric identifiers always have lower precedence than non-numeric identifiers

Builds are ignored when calculating precedence: version 1.2.3 and 1.2.3+build5 are considered equal.

