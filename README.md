# ets-kv

[![Build Status][gh-actions-badge]][gh-actions]
[![Erlang Versions][erlang-badge]][versions]
[![Tags][github-tags-badge]][github-tags]

*A simple in-memory K/V store with MVCC semantics for Erlang/BEAM languages*

[![][logo]][logo-large]

##### Contents

* [Introduction](#introduction-)
* [Installation](#installation-)
* [Usage](#usage-)
* [License](#license-)

## Overview [&#x219F;](#contents)

The ets-kv project (originally "memdb") offers BEAM languages a simple K/V store built on top of [ETS](http://www.erlang.org/doc/man/ets.html). In addition to ETS features and capabilities, ets-kv provides concurrent access to the store using [MVCC](https://en.wikipedia.org/wiki/Multiversion_concurrency_control), allowing multiple clients to read the store concurrently, getting a consitent view of the store without locking.

All writes are serialized for now.

Note that ets-kv's memory consumption increases monotonically, even if keys are deleted or values are updated. Compaction can be
triggered manually or automatically using the auto-vacuum. A full snapshot can be stored or copied in another store
if needed.

## Build [&#x219F;](#contents)

```shell
$ rebar3 compile
```

## Usage


In the directory of a clone of this repo, start up an Erlang shell:

``` shell
$ rebar3 shell
```

### Create a Store [&#x219F;](#contents)

```erl
Name = mystor.
Store = etskv:open(Name).
```

### Add a value [&#x219F;](#contents)

Add a value associated with a key using `etskv:put/3`:

```erl
Key = <<"a">>,
Value = 1,
ok =  etskv:put(Key, Value, Store).
```

### Retrieve a value [&#x219F;](#contents)

Use the `etskv:get/2` function to retrieve a value.

```erl
Value = etskv:get(Key, Store).
```

Value should be `1`. Note that you can use `etskv:contains/2` to check ahead of time:

``` erl
etskv:contains(Key, Store).
```

### Delete a value [&#x219F;](#contents)

Use `etskv:delete/2` to delete a value:

```erl
ok = etskv:delete(Key, Store).
```

### Working with Multiple Values [&#x219F;](#contents)

#### Storing

Using `etskv:batch/2` you can write and delete multiple values in one
pass:

```erl
ok =  etskv:batch([{put, <<"a">>, 1},
                   {put, <<"b">>, 2},
                   {put, <<"c">>, 3}], Store),

ok =  etskv:batch([{put, <<"d">>, 4},
                   {del, <<"b">>},
                   {put, <<"e">>, 5}], Store).
```

#### Retrieving

Use `etskv:fold/4` to retrieve multiples K/Vs

### Close the Store [&#x219F;](#contents)

Close a storage using `etskv:close/1`:

```erl
etskv:close(Store)
```

## License [&#x219F;](#contents)

```
Copyright © 2015 Benoît Chesneau
Copyright © 2024 Duncan McGreggor

Distributed under the Mozilla Public License Version 2.0.
```

[//]: ---Named-Links---

[logo]: priv/images/logo.png
[logo-large]: priv/images/logo-large.jpg
[gh-actions-badge]: https://github.com/erlsci/ets-kv/workflows/ci%2Fcd/badge.svg
[gh-actions]: https://github.com/erlsci/ets-kv/actions
[erlang-badge]: https://img.shields.io/badge/erlang-21%20to%2026-blue.svg
[versions]: https://github.com/erlsci/ets-kv/blob/master/.github/workflows/cicd.yml
[github-tags]: https://github.com/erlsci/ets-kv/tags
[github-tags-badge]: https://img.shields.io/github/tag/erlsci/ets-kv.svg
