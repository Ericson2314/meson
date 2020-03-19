## Read standard environment variables in more cases

Make variables `CC_FOR_BUILD` work in native builds too. [Config files, which
one should greatly prefer over environment variables, still take precedence,
however.]

As of version 0.??, variables like `CC_FOR_BUILD` controls the build platform
options (if not set by a config file) in cross builds. But it is sometimes
useful to have a separate config for `native: true` items in native builds too,
even though it is not needed. (An example could be to avoid optimizing
intermediate artifacts which won't be installed.)

By making `CC_FOR_BUILD` and friends effect the build platform in native and
cross builds alike, we remove special casing and hew to the Autoconf and other
tools standard more closely. We do leave in place that variables like `CC` also
affect the build platform in native builds if `CC_FOR_BUILD` is not set, as
this is also standard.
