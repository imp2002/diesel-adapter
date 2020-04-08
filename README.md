# diesel-adapter

[![Crates.io](https://img.shields.io/crates/v/diesel-adapter.svg)](https://crates.io/crates/diesel-adapter)
[![Docs](https://docs.rs/diesel-adapter/badge.svg)](https://docs.rs/diesel-adapter)
[![Build Status](https://travis-ci.org/casbin-rs/diesel-adapter.svg?branch=master)](https://travis-ci.org/casbin-rs/diesel-adapter)

An adapter designed to work with [casbin-rs](https://github.com/casbin/casbin-rs).


## Install

Add it to `Cargo.toml`

```
casbin = { version = "0.4.3" }
diesel-adapter = { version = "0.4.3", features = ["postgres"] }
async-std = "1.5.0"
```


## Example

```rust
use casbin::prelude::*;
use diesel_adapter::{DieselAdapter, ConnOptions};

#[async_std::main]
async fn main() -> Result<()> {
    let mut m = DefaultModel::from_file("examples/rbac_model.conf").await?;

    let mut conn_opts = ConnOptions::default();
    conn_opts
        .set_hostname("127.0.0.1")
        .set_port(5432)
        .set_host("127.0.0.1:5433") // overwrite hostname, port config
        .set_database("casbin")
        .set_auth("casbin_rs", "casbin_rs");

    let a = DieselAdapter::new(conn_opts)?;
    let mut e = Enforcer::new(m, a).await?;
    Ok(())
}
```

## Features

- `postgres`
- `mysql`

*Attention*: `postgres` and `mysql` are mutual exclusive which means that you can only activate one of them. Currently we don't have support for `sqlite`, it may be added in the near future.
