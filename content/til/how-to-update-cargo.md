---
title: "How to Update Cargo"
date: 2022-10-24T15:30:42+02:00
tags:
- rust
- cargo
---

Today I saw a [Tweet](https://twitter.com/mgedmin/status/1583421996167487488) by Marius Gedminas,
who wrote about [lfs](https://github.com/Canop/lfs),
a replacement for `df`.

It is one of the many tools, implemented in Rust,
which offer some more features compared to their bash alternatives.

## What about no

When I tried to install it,
I just saw the following error message:

```
❯ cargo install lfs
    Updating crates.io index
  Downloaded lfs v2.6.0
error: failed to parse manifest at `/home/jugmac00/.cargo/registry/src/github.com-1ecc6299db9ec823/lfs-2.6.0/Cargo.toml`

Caused by:
  feature `strip` is required

  The package requires the Cargo feature called `strip`, but that feature is not stabilized in this version of Cargo (1.56.0 (4ed5d137b 2021-10-04)).
  Consider trying a newer version of Cargo (this may require the nightly release).
  See https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#profile-strip-option for more information about the status of this feature.
```

While the error message is pretty nice,
it lacks one important thing: "How do I update Cargo?"

But first I checked the currently installed version:

```
❯ cargo --version
cargo 1.56.0 (4ed5d137b 2021-10-04)
```

Googling for `update cargo` did not really help a lot,
as the results mostly referred to the `cargo update` command.

## What about rustup

After a while I found the correct command:

```
rustup update
```

After running the above command,
`cargo` was updated and I finally was able to install `lfs`.

```
❯ cargo --version
cargo 1.64.0 (387270bc7 2022-09-16)
```

```
❯ cargo install lfs
    Updating crates.io index
  Installing lfs v2.6.0
  Downloaded argh_shared v0.1.9
...
   Compiling lfs v2.6.0
    Finished release [optimized] target(s) in 24.19s
  Installing /home/jugmac00/.cargo/bin/lfs
   Installed package `lfs v2.6.0` (executable `lfs`)
```

## Still here?

Right, why would you install `lfs` when `df` is available?

SNAPS! :-)

Try to run `df` on any Ubuntu system (< 22.04) and you will see a lot of noise from installed Snaps.

`lfs` on the other hand filters Snap out by default.


## Update

### 2022-10-24

My colleague Oliver Grawert pointed out that beginning with 22.04,
`df` filters Snaps, too!
