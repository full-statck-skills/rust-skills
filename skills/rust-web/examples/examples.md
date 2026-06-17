# Web Examples

## Minimal axum server
```rust
use axum::{Router, routing::get, response::Json};

async fn hello() -> Json<&'static str> {
    Json("Hello, World!")
}

#[tokio::main]
async fn main() {
    let app = Router::new().route("/", get(hello));
    let listener = tokio::net::TcpListener::bind("0.0.0.0:3000").await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

## CRUD with state
```rust
use axum::{Router, extract::{Path, State}, response::Json, routing::get};
use std::sync::Arc;
use tokio::sync::Mutex;

type Db = Arc<Mutex<Vec<String>>>;

async fn list(State(db): State<Db>) -> Json<Vec<String>> {
    Json(db.lock().await.clone())
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/items", get(list))
        .with_state(Arc::new(Mutex::new(vec![])));
    let listener = tokio::net::TcpListener::bind("0.0.0.0:3000").await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

## reqwest client
```rust
let client = reqwest::Client::new();
let resp = client.get("https://api.github.com/users")
    .header("User-Agent", "my-app")
    .send()
    .await?;
let body: serde_json::Value = resp.json().await?;
```
