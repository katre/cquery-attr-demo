# Test cquery and attr.

## Basic query

Expect to get two targets, `//:no_select_copts_present` and `//:select_copts`,
because `query` includes all `select` branches.

```
$ bazel query 'attr(copts, "-Wno.*", //:all)'
//:no_select_copts_present
//:select_copts
```

## Basic cquery - flag not set.

Expect to get one target, `//:no_select_copts_present`, because `cquery` is
configuration-aware.

```
$ bazel cquery 'attr(copts, "-Wno.*", //:all)'
//:no_select_copts_present (5b7f3da)
//:select_copts (5b7f3da)
```

This looks wrong.

## Basic cquery - flag set.

Expect same result.

```
$ bazel cquery --//:a=true 'attr(copts, "-Wno.*", //:all)'
//:no_select_copts_present (270ac80)
//:select_copts (270ac80)
```

Also wrong.

## Basic cquery - explicitly disable flag.

Should be the same as not setting the flag.

```
$ bazel cquery --//:a=false 'attr(copts, "-Wno.*", //:all)'
//:no_select_copts_present (5b7f3da)
//:select_copts (5b7f3da)
```

Also wrong.

