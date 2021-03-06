# Lapin integration with async-io

This crate integrates lapin with async-io by using async-io's reactor inside of lapin.

```
use async_lapin::*;
use lapin::{executor::Executor, Connection, ConnectionProperties, Result};
use std::{future::Future, pin::Pin};

fn main() -> Result<()> {
    async_global_executor::block_on(async {
        let addr = std::env::var("AMQP_ADDR").unwrap_or_else(|_| "amqp://127.0.0.1:5672/%2f".into());
        let conn = Connection::connect(
            &addr,
            ConnectionProperties::default().with_async_io(SmolExecutor)
        )
        .await?; // Note the `with_async_io()` here
        let channel = conn.create_channel().await?;

        // Rest of your program
    })
}
```
