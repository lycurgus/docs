+++
title = "Eyre Public API"
weight = 4
template = "doc.html"
+++

# Eyre

In this document we describe the public interface for Eyre. Namely, we describe
each `task` that Eyre can be `%pass`ed, and which `gift`(s) Eyre can `%give` in return.


## Tasks

### `%born`

Each time you start your Urbit, the Arvo kernel calls the `%born` task for Eyre.
This `task` closes all previously open connections.

#### Accepts

```hoon
[~]
```

#### Returns

```hoon
[%set-config =http-config]
```

Eyre `%give`s a `%set-config` `gift` containing the default http configuration.


### `%cancel-request`

This `task` cancels a previous `%request`. Which `%request` to cancel is
inferred from the duct. 

#### Accepts

```hoon
[~]
```

#### Returns

What this `task` returns depends on the type of `%request` being canceled.

Associated to the `duct` is an `$outstanding-connection`, which contains an
`$action` that is in queue to be performed. The return depends on the value of the `$action`.

If the connection is empty, nothing has yet handled it and so Eyre does nothing.

A `%gen` action triggers the following `move`:
```hoon
[%pass /run-build %f %kill ~]
```

For a `%app` action,
```hoon
[%pass /watch-response/[eyre-id] %g %deal [our our] app.action.u.connection %leave ~]
```

For a `%authentication` action, Eyre returns nothing.

For a `%channel` action, Eyre cancels the subscription associated to the duct
but does not return any gift (I think?)

For a `%four-oh-four` action, Eyre crashes as it should be impossible for a 404
page to be asynchronous.


### `%connect`

#### Accepts

#### Returns


### `%crud`

#### Accepts

#### Returns


### `%disconnect`

#### Accepts

#### Returns


### `%init`

`%init` is called a single time during the very first boot process, immediately
after the [larval stage](@/docs/tutorials/arvo/arvo.md#larval-stage-core)
is completed. This initializes the vane. Jael is initialized first, followed by
other vanes such as Eyre.

In response to receiving the `%init` `task`, Ames sets the initial values for
the login handler.

#### Accepts

```hoon
[p=ship]
```
The argument with `%init` is always the `@p` of the ship, but Eyre does not make
use of it.

#### Returns

This `task` returns no `gift`s.


### `%live`

#### Accepts

#### Returns


### `%request`

#### Accepts

#### Returns


### `%request-local`

#### Accepts

#### Returns


### `%rule`

#### Accepts

#### Returns


### `%serve`

#### Accepts

#### Returns


### `%trim`

#### Accepts

#### Returns


### `%vega`

`%vega` is called whenever the kernel is updated. Ames currently does not do
anything in response to this.

#### Accepts

`%vega` takes no arguments.

#### Returns

This `task` returns no `gift`s.


### `%wegh`

This `task` is a request to Eyre to produce a memory usage report.

#### Accepts

This `task` has no arguments.

#### Returns

In response to this `task,` Eyre `%give`s a `%mass` `gift` containing Eyre's
current memory usage.