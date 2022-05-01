# openai-api-rust-fork

A simple rust client for OpenAI API.

Has a few conveniences, but is mostly at the level of the API itself.

### NOTE!

This is a fork of openai-api. This fork removes the sync and async features and only provides an async api. Also, surf was removed because the maintainer of http-rs is unresponsive. This fork now uses reqwest which is well maintained by the author of hyper.

# Installation

```
$ cargo add openai-api-fork
```

# Quickstart

```rust
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let api_token = std::env::var("OPENAI_SK")?;
    let client = openai_api_fork::Client::new(&api_token);
    let prompt = String::from("Once upon a time,");
    println!(
        "{}{}",
        prompt,
        client.complete(prompt.as_str()).await?
    );
    Ok(())
}
```
# Basic Usage

## Creating a completion

For simple demos and debugging, you can do a completion and use the `Display` instance of a `Completion` object to convert it to a string:

```rust
let response = client.complete_prompt("Once upon a time").await?;
println!("{}", response);
```

To configure the prompt more explicitly, you can use the `CompletionArgs` builder:

```rust
let args = openai_api_fork::api::CompletionArgs::builder()
        .prompt("Once upon a time,")
        .engine("davinci")
        .max_tokens(20)
        .temperature(0.7)
        .top_p(0.9)
        .stop(vec!["\n".into()]);
let completion = client.complete_prompt(args).await?;
println!("Response: {}", response.choices[0].text);
println!("Model used: {}", response.model);
```

See [examples/](./examples)
