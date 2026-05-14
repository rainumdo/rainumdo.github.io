# 18

`dioxus motion`

# 17

`stop_propagation`

```
rsx! {
    button {
        onclick: move |evt: Event<MouseData>| {
            evt.stop_propagation();
        }
    }
};
```

# 16

`ErrorBoundary`

```
use dioxus::prelude::*;

fn App() -> Element {
    let mut multiplier = use_signal(|| String::from("2"));
    rsx! {
        input {
            r#type: "text",
            value: multiplier,
            oninput: move |e| multiplier.set(e.value())
        }
        ErrorBoundary {
            handle_error: |errors: ErrorContext| {
                rsx! {
                    div {
                        "Oops, we encountered an error. Please report {errors:?} to the developer of this application"
                    }
                }
            },
            Counter {
                multiplier
            }
        }
    }
}

#[component]
fn Counter(multiplier: ReadSignal<String>) -> Element {
    let multiplier_parsed = multiplier().parse::<usize>()?;
    let mut count = use_signal(|| multiplier_parsed);
    rsx! {
        button {
            onclick: move |_| {
                let multiplier_parsed = multiplier().parse::<usize>()?;
                *count.write() *= multiplier_parsed;
                Ok(())
            },
            "{count}x{multiplier}"
        }
    }
}
```

# 15

```
document::Link { rel: "icon", herf: asset!("xxx")}

document::Title { "xxx app"}
```

# 14

`wasm_bindgen` import and call javascript functions

# 13

browser log

```
web-sys::console::log_1("web browser log print");
```

# 12

| 写法|作用|匹配示例|类型|
| --- | --- | --- | --- |
| /:id|单段捕获|/user/123|String / u32|
|/:id?|可选单段|/blog/blog/5|Option<T>|
|/:..paths| 全路径捕获| /a/b/c| Vec<String> |
|/:.._ |匿名全捕获| 所有路径| 无参数 |
|/files/:..p| 前缀 + 全捕获 |/files/x/y/z |Vec<String>|

# 11

`Link`

```
#[derive(Clone, Routable)]
enum Route {
    #[route("/")]
    Index {},
}

#[component]
fn App() -> Element {
    rsx! {
        Router::<Route> {}
    }
}

#[component]
fn Index() -> Element {
    rsx! {
        Link {
            active_class: "active",
            class: "link_class",
            id: "link_id",
            new_tab: true,
            rel: "link_rel",
            to: Route::Index {},

            "A fully configured link"
        }
    }
}
```

# 10

[Rust 宏语法规定：( ) [ ] { } 三种括号完全等价](https://rustwiki.org/zh-CN/reference/macros.html)

# 9

`use_navigator`

```rust
use dioxus::prelude::*;

#[derive(Clone, Routable)]
enum Route {
    #[route("/")]
    Index {},
    #[route("/:id")]
    Dynamic { id: usize },
}

#[component]
pub fn App() -> Element {
    rsx! {
        Router::<Route> {}
    }
}

#[component]
fn Index() -> Element {
    let navigator = use_navigator();

    rsx! {
        button {
            onclick: move |_| { navigator.push(Route::Dynamic { id: 1234 }); },
            "Go to /1234"
        }
    }
}

#[component]
fn Dynamic(id: usize) -> Element {
    rsx! {
        p {
            "Current ID: {id}"
        }
    }
}
```

# 8

`use_route`

```rust
use dioxus::prelude::*;

#[derive(Clone, Routable)]
enum Route {
    #[route("/")]
    Index {},
}

#[component]
pub fn App() -> Element {
    rsx! {
        h1 { "App" }
        Router::<Route> {}
    }
}

#[component]
fn Index() -> Element {
    let path: Route = use_route();
    rsx! {
        h2 { "Current Path" }
        p { "{path}" }
    }
}
```

# 7

`use_memo`

```rust
use dioxus::prelude::*;

#[component]
pub fn App() -> Element {
    let mut count = use_signal(|| 0);
    // the double memo will always be equal to two times the value of count, even after count changes
    let double = use_memo(move || count * 2);

    rsx! {
        p{"{double}"}

        button {
            // When count changes, the memo will rerun and double will be updated
            // memos rerun any time you write to a signal they read. They will only rerun values/component that depend on them if the value of the memo changes
            onclick: move |_| count += 1,
            "Increment"
        }
    }
}
```

# 6

`use_resource`

```rust
use dioxus::prelude::*;

async fn get_weather(location: &WeatherLocation) -> Result<String, String> {
    Ok("Sunny".to_string())
    // Err("404".to_string())
}

#[component]
pub fn App() -> Element {
    let country = use_signal(|| WeatherLocation {
        city: "Berlin".to_string(),
        country: "Germany".to_string(),
        coordinates: (52.5244, 13.4105),
    });

    // Because the resource's future subscribes to `country` by reading it (`country.read()`),
    // every time `country` changes the resource's future will run again and thus provide a new value.
    let current_weather = use_resource(move || async move { get_weather(&country()).await });

    rsx! {
        // the value of the resource can be polled to
        // conditionally render elements based off if it's future
        // finished (Some(Ok(_)), errored Some(Err(_)),
        // or is still running (None)
        match &*current_weather.read_unchecked() {
            Some(Ok(weather)) => rsx! { WeatherElement { weather } },
            Some(Err(e)) => rsx! { p { "Loading weather failed, {e}" } },
            None =>  rsx! { p { "Loading..." } }
        }
    }
}

#[derive(Clone)]
struct WeatherLocation {
    city: String,
    country: String,
    coordinates: (f64, f64),
}

#[component]
fn WeatherElement(weather: String) -> Element {
    rsx! { p { "The weather is {weather}" } }
}
```

# 5

`use_context_provider`

```rust
use dioxus::prelude::*;

#[derive(Clone)]
struct ThemeState {
    is_dark: bool,
}

#[component]
pub fn App() -> Element {
    let mut theme = use_signal(|| ThemeState { is_dark: false });

    use_context_provider(|| theme);

    rsx! {
        Child {  }
    }
}

#[component]
fn Child() -> Element {
    let mut theme = use_context::<Signal<ThemeState>>();
    rsx! {
        p { "theme:" {if theme().is_dark { "dark" } else { "light" }} }
        button {
            onclick: move |_| theme.write().is_dark = !theme().is_dark,
            "switch"
        }
    }
}
```

# 4

`use_effect` and `oninput`

```rust
use dioxus::{logger::tracing, prelude::*};

#[component]
pub fn App() -> Element {
    let mut input_val = use_signal(|| String::new());

    use_effect(move || {
        tracing::debug!("输入框内容：{}", input_val());
    });

    rsx! {
        input {
            value: "{input_val}",
            oninput: move |e| input_val.set(e.value()),
            placeholder: "随便输点什么"
        }
    }
}
```

# 3

`use_signal`

```rust
use dioxus::prelude::*;

#[component]
pub fn App() -> Element {
    let mut count = use_signal(|| 0);

    rsx! {
        div {
            padding: "20px",

            h1 {"count:{count}"}
            button {
                onclick: move |_| count+=1, "+1"
            }
            button {
                onclick: move |_| count-=1, "-1"
            }
        }
    }
}
```

# 2

`use_hook`

```rust
use dioxus::prelude::*;

#[component]
pub fn App() -> Element {
    let count = use_hook(|| "dioxus");

    rsx! {
        div { {count} }
    }
}
```

# 1

```
cargo install dioxus-cli
```
