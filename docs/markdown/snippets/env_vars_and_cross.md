## Read standard environment variables in more cases

Make variables like `CC` and `CC_FOR_BUILD` never be ignored, regardless of
whether the overall build is native of cross. [Config files, which one should
greatly prefer over environment variables, still take precedence, however.]

As of version 0.54, variables like `CC` controls the host platform options (if
not set by a config file) in native builds, while variables like `CC_FOR_BUILD`
does it for the build platform in cross builds. The intent was to limit the use
of environment variables to a bare minimum and encourage putting everything in
config files.

However, the special-casing of cross vs native means infrastructure around
meson packages like distro packages is harder to test, and cross builds in
particular are more likely to bit-rot if native builds are exclusively tested,
as is often the case.

By making `CC` always control the host platform, and `CC_FOR_BUILD` for build
the native platform, we remove much special casing. We do leave in place that
variables like `CC` also affect the build platform in native builds if
`CC_FOR_BUILD` is not set, as this is the standard.

As an added bonus, this change, including the `build = host` fallback rules
just described, brings us in full conformance with how Autoconf reads
environment variables.
