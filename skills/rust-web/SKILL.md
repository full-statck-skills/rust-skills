---
name: rust-web
description: Rust Web 开发技能 — 涵盖主流 Web 框架（axum、actix-web、rocket、tower）、异步 HTTP 客户端、WebSocket、中间件、数据库驱动、序列化/反序列化、静态文件服务、模板引擎、测试工具。当用户需要构建 Rust Web 应用或 API 服务时激活。
---

# Rust Web 开发

> Rust Web 生态指南，基于 community 主要 crate 的实践总结。

## Capability Boundaries

### ✅ 强项
1. 路由与请求处理（axum、actix-web、rocket）
2. 中间件（tower、自定义中间件）
3. 状态共享（axum State、actix App Data）
4. JSON 序列化（serde + serde_json）
5. 数据库集成（sqlx、sea-orm、diesel）
6. WebSocket（axum WebSocket、tungstenite）
7. 表单处理与文件上传
8. 静态文件服务（axum ServeDir、actix_files）
9. 模板渲染（askama、tera、minijinja）
10. API 测试（reqwest、cargo test with server）

### ⚠️ 前置要求
1. 理解 async/await 编程模型

### ❌ 不适用范围
1. Rust 基础语法 → 使用 `rust-core` 技能
2. 并发原理解释 → 使用 `rust-concurrency` 技能

## 何时使用

- "创建一个 REST API"
- "用 axum/actix 写 Web 服务"
- "连接数据库"
- "处理 HTTP 请求"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Rust Web 开发参考

## axum（推荐，tokio 生态）

```rust
use axum::{
    Router,
    extract::{Path, Query, State},
    http::StatusCode,
    response::Json,
    routing::get,
};
use serde::{Deserialize, Serialize};
use std::sync::Arc;
use tokio::sync::Mutex;

#[derive(Serialize, Deserialize)]
struct User {
    id: u64,
    name: String,
}

type Db = Arc<Mutex<Vec<User>>>;

async fn list_users(State(db): State<Db>) -> Json<Vec<User>> {
    let users = db.lock().await;
    Json(users.clone())
}

#[tokio::main]
async fn main() {
    let db: Db = Arc::new(Mutex::new(vec![]));
    
    let app = Router::new()
        .route("/users", get(list_users).post(create_user))
        .route("/users/:id", get(get_user))
        .layer(tower::ServiceBuilder::new());

    let listener = tokio::net::TcpListener::bind("0.0.0.0:3000").await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

## 数据库（sqlx）

```toml
# Cargo.toml
[dependencies]
sqlx = { version = "0.8", features = ["runtime-tokio", "postgres", "mysql", "sqlite"] }
```

```rust
use sqlx::postgres::PgPoolOptions;

#[derive(sqlx::FromRow, serde::Serialize)]
struct User {
    id: i64,
    name: String,
    email: String,
}

async fn query_users(pool: &sqlx::PgPool) -> Result<Vec<User>, sqlx::Error> {
    sqlx::query_as::<_, User>("SELECT id, name, email FROM users")
        .fetch_all(pool)
        .await
}
```

## 中间件模式

```rust
use tower::ServiceBuilder;
use tower_http::{
    cors::CorsLayer,
    trace::TraceLayer,
    compression::CompressionLayer,
};

let app = Router::new()
    .route("/api/:path", get(handler))
    .layer(
        ServiceBuilder::new()
            .layer(TraceLayer::new_for_http())
            .layer(CompressionLayer::new())
            .layer(CorsLayer::permissive())
    );
```

## 常用 Crate

| 用途 | crate | 说明 |
|------|-------|------|
| Web 框架 | axum | 基于 tokio/tower，推荐 |
| Web 框架 | actix-web | actix 运行时 |
| Web 框架 | rocket | 强调易用性 |
| HTTP 客户端 | reqwest | async HTTP client |
| 序列化 | serde / serde_json | JSON 序列化 |
| 数据库 | sqlx | 编译时检查 SQL |
| ORM | sea-orm | 异步 ORM |
| 模板 | askama | 编译时模板 |
| WebSocket | axum::extract::ws | 内置 WebSocket |
