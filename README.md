# Shepherd

Shepherd is the last line of defense before production.


## Introduction

In most modern companies, code is shipped daily. Hopefully, it goes through a rigourous code review process. One of the problems though with code reviews is that the reviewers are humans. We cannot possible be expected to catch every nuance.

Code reviews can be trivial -- "please trim the whitespace" -- to the wasted brain cells -- "a trailing comma in an array breaks in IE8".

Whatever the case, Shepherd protects against it before it hits production.

## How it works

Shepherd simply runs yet another unit test on your CI (you do have a CI don't you?). The test is defined by anything you wish to config. If any of the configs are not satisfied, the test fails. If everything checks out, the build is good to go to production.

## Config

### No touchy

No-touchy is for that sensitive or hard to refactor code.

```
{
  "no-touchy": [
  	"/path/to/global.css": {
  		"message": "Please do not add to the global stylesheet. Instead, create module specific stylesheets to avoid CSS pollution."
  	},
  	"/path/to/file-b": {
  		"message": "file-b is deprecated, use file-c instead",
  		"ref": "http://link-to-internal.doc"
  	}
  ]
}
```

### Linters

Linters come in a variety of flavors, but they all end up with the same result: reporting the number of violations a file has.

While zero violations is ideal, sometimes is not always possible. One doesn't want to change code that doesn't affect their commit else they may be `git blame`d in the future. Therefore, all linters have a configurable `adjustment` option which can be `simple` or `aggressive`.

| Value      | Description                                                         |
| ---------- | ------------------------------------------------------------------- |
| simple     | The code change must decrease the violations or keep it the same.   |
| aggressive | The code change must ensure the violations goes to or stays at zero |

The default value for all linters is `simple`.
