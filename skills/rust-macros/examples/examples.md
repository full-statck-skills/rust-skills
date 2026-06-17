# Macro Examples

## Declarative macro
```rust
macro_rules! my_vec {
    ($($x:expr),* $(,)?) => {{
        let mut v = Vec::new();
        $(v.push($x);)*
        v
    }};
}
let v = my_vec![1, 2, 3];
```

## Derive macro (proc)
```rust
// In lib.rs of a proc-macro crate
#[proc_macro_derive(Hello)]
pub fn hello_derive(input: TokenStream) -> TokenStream {
    let input = parse_macro_input!(input as DeriveInput);
    let name = &input.ident;
    quote! { impl Hello for #name { fn hello() { println!("Hello!"); } } }.into()
}
```

## Logging derive
```rust
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, DeriveInput};

#[proc_macro_derive(Loggable)]
pub fn loggable(input: TokenStream) -> TokenStream {
    let DeriveInput { ident, .. } = parse_macro_input!(input);
    quote! {
        impl Loggable for #ident {
            fn log(&self) { println!("Logging {}", stringify!(#ident)); }
        }
    }.into()
}
```
