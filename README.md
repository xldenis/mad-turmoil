# mad-turmoil

[madsim](https://github.com/madsim-rs/madsim)-inspired determinism for [turmoil](https://github.com/tokio-rs/turmoil)-based simulation tests

See [blog post](https://s2.dev/blog)

## Setup

Make sure you are only depending on this crate for simulation binaries!

```rust
fn main() -> eyre::Result<()> {
    let rng_seed = std::env::var("DST_SEED")?.parse()?;

    // Taming randomness...

    let mut rng = StdRng::seed_from_u64(rng_seed);
    mad_turmoil::rand::set_rng(rng.clone());
    assert_eq!(rng.next_u64(), mad_turmoil::rand::get_rng().next_u64());

    // Additionally, if you are using fastrand (possibly transitively!)
    fastrand::seed(rng_seed);

    // Taming time...

    let _tokio_time_guard = mad_turmoil::time::SimClocksGuard::init();

    // Go forth and create turmoil...
}
```
