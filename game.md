# 79

Audio

```rust
#[derive(Component, Reflect, Default)]
#[reflect(Component)]
pub struct SoundEffect;

pub fn sound_effect(handle: Handle<AudioSource>) -> impl Bundle {
    (AudioPlayer(handle), PlaybackSettings::DESPAWN, SoundEffect)
}
```

# 78

Asset Loading Demo

```rust
#[derive(Resource, Asset, Clone, Reflect)]
#[reflect(Resource)]
struct ActorAssets {
    // This #[dependency] attribute marks the field as a dependency of the Asset.
    // This means that it will not finish loading until the labeled asset is also loaded.
    #[dependency]
    player: Handle<Image>,
    #[dependency]
    enemies: Vec<Handle<Image>>,
}

impl FromWorld for ActorAssets {
    fn from_world(world: &mut World) -> Self {
        let assets = world.resource::<AssetServer>();
        Self {
            player: assets.load("images/player.png"),
            enemies: vec![
                assets.load("images/enemy1.png"),
                assets.load("images/enemy2.png"),
                assets.load("images/enemy3.png"),
            ],
        }
    }
}
```

# 77

Bundle Function Demo

```rust
pub fn monster(health: u32, transform: Transform) -> impl Bundle {
    (
        Name::new("Monster"),
        Health::new(health),
        transform,
        // other components
    )
}

```

# 76

State Demo

```rust
// victory.rs
pub(super) fn plugin(app: &mut App) {
    app.add_systems(OnEnter(Screen::Victory), spawn_victory_screen);
    app.add_systems(OnExit(Screen::Victory), reset_highscore);
}

fn spawn_victory_screen(mut commands: Commands) {
    commands.spawn((
        widget::ui_root("Victory Screen"),
        DespawnOnExit(Screen::Victory),
        children![
            // UI elements.
        ],
    ));
}

fn reset_highscore(mut highscore: ResMut<Highscore>) {
    *highscore = default();
}
```

# 75

- AudioSource: data
- AudioPlayer: stream
- AudioSink: receiver

# 74

[align_content](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/align-content)  
[align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/align-items)  
[justify-content](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/justify-content)  
[justify-items](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/justify-items)  
content: multiple line as a whole  
items: flexbox => cross axis, gridbox => block axis  

# 73

1. window.cursor_position
2. camera.viewport_to_world_2d

```rust
fn draw_cursor(
    camera_query: Single<(&Camera, &GlobalTransform)>,
    window: Single<&Window>,
    mut gizmos: Gizmos,
) {
    let (camera, camera_transform) = *camera_query;

    if let Some(cursor_position) = window.cursor_position()
        // Calculate a world position based on the cursor's position.
        && let Ok(world_pos) = camera.viewport_to_world_2d(camera_transform, cursor_position)
        // To test Camera::world_to_viewport, convert result back to viewport space and then back to world space.
        && let Ok(viewport_check) = camera.world_to_viewport(camera_transform, world_pos.extend(0.0))
        && let Ok(world_check) = camera.viewport_to_world_2d(camera_transform, viewport_check.xy())
    {
        gizmos.circle_2d(world_pos, 10., WHITE);
        // Should be the same as world_pos
        gizmos.circle_2d(world_check, 8., RED);
    }
}
```

# 72

1. init_resource
2. from_world

```
use bevy::prelude::*;
use bevy_ecs_tilemap::prelude::*;
use rand::{rng, seq::IndexedRandom};

pub fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(TilemapPlugin)
        .init_resource::<TileHandleIos>()
        .add_systems(Startup, spawn_tilemap)
        .run();
}

fn spawn_tilemap(mut commands: Commands, tile_handle: Res<TileHandleIos>) {
    commands.spawn(Camera2d);

    let map_size = TilemapSize {
        x: MAP_SIDE_LENGTH_X,
        y: MAP_SIDE_LENGTH_Y,
    };

    let mut tile_storage = TileStorage::empty(map_size);
    let tilemap_entity = commands.spawn_empty().id();
    let tilemap_id = TilemapId(tilemap_entity);

    fill_tilemap(
        TileTextureIndex(0),
        map_size,
        tilemap_id,
        &mut commands,
        &mut tile_storage,
    );

    let mut rng = rng();
    let colors: Vec<Color> = vec![
        color("FFBE0B"),
        color("FB5607"),
        color("FF006E"),
        color("8338EC"),
        color("3A86FF"),
    ];
    for tile_id in tile_storage.iter().flatten() {
        commands
            .entity(*tile_id)
            .insert(TileColor(*colors.choose(&mut rng).unwrap()));
    }

    let tile_size = TILE_SIZE_ISO;
    let grid_size = GRID_SIZE_ISO;
    let map_type = TilemapType::Isometric(IsoCoordSystem::Diamond);

    commands.entity(tilemap_entity).insert(TilemapBundle {
        grid_size,
        size: map_size,
        storage: tile_storage,
        texture: TilemapTexture::Single(tile_handle.clone()),
        tile_size,
        map_type,
        anchor: TilemapAnchor::Center,
        ..Default::default()
    });
}

#[derive(Deref, Resource)]
struct TileHandleIos(Handle<Image>);

impl FromWorld for TileHandleIos {
    fn from_world(world: &mut World) -> Self {
        let asset_server = world.resource::<AssetServer>();
        Self(asset_server.load("bevy_ecs_tilemap/bw-tile-iso.png"))
    }
}

const MAP_SIDE_LENGTH_X: u32 = 10;
const MAP_SIDE_LENGTH_Y: u32 = 10;
const TILE_SIZE_ISO: TilemapTileSize = TilemapTileSize { x: 100.0, y: 50.0 };
const GRID_SIZE_ISO: TilemapGridSize = TilemapGridSize { x: 100.0, y: 50.0 };

fn color(s: &str) -> Color {
    Srgba::hex(s).expect("hex color").into()
}
```

# 71

3D Animation

```rust
use bevy::{light::CascadeShadowConfigBuilder, prelude::*};
use bevy_inspector_egui::{bevy_egui::EguiPlugin, quick::WorldInspectorPlugin};

pub fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(EguiPlugin::default())
        .add_plugins(WorldInspectorPlugin::default())
        .add_systems(Startup, setup)
        .add_systems(First, add_animation_graph)
        .add_systems(Update, toggle_animation)
        .run();
}

const GLTF_PATH: &str = "gltf/violet_dimension_breaker_aov.glb";

fn setup(mut commands: Commands, asset_server: Res<AssetServer>) {
    commands.spawn((
        Camera3d::default(),
        Transform::from_xyz(0.0, 0.0, 10.).looking_at(Vec3::ZERO, Vec3::Y),
    ));

    commands.spawn((
        DirectionalLight {
            shadows_enabled: true,
            ..default()
        },
        CascadeShadowConfigBuilder {
            num_cascades: 1,
            maximum_distance: 1.6,
            ..default()
        }
        .build(),
    ));

    let (graph, nodes) = AnimationGraph::from_clips([
        asset_server.load(GltfAssetLabel::Animation(0).from_asset(GLTF_PATH)),
        asset_server.load(GltfAssetLabel::Animation(1).from_asset(GLTF_PATH)),
    ]);

    commands.insert_resource(Animations {
        graph_handle: asset_server.add(graph),
        nodes,
    });

    commands.spawn((
        SceneRoot(asset_server.load(GltfAssetLabel::Scene(0).from_asset(GLTF_PATH))),
        Transform::from_xyz(0., -2.5, 0.),
    ));
}

#[derive(Resource)]
struct Animations {
    graph_handle: Handle<AnimationGraph>,
    nodes: Vec<AnimationNodeIndex>,
}

fn add_animation_graph(
    mut commands: Commands,
    entities: Query<Entity, With<AnimationPlayer>>,
    animations: Res<Animations>,
) {
    for entity in entities {
        commands
            .entity(entity)
            .insert(AnimationGraphHandle(animations.graph_handle.clone()));
    }
}

fn toggle_animation(
    keyboard: Res<ButtonInput<KeyCode>>,
    players: Query<&mut AnimationPlayer>,
    animations: Res<Animations>,
) {
    for mut player in players {
        if keyboard.just_pressed(KeyCode::Digit1) {
            player.stop_all();
            player.play(animations.nodes[0]);
        } else if keyboard.just_pressed(KeyCode::Digit2) {
            player.stop_all();
            player.play(animations.nodes[1]);
        }
    }
}

```

# 70

[sketchfab](https://sketchfab.com)

# 69

法线贴图的本质  
法线贴图是一张存储「法线向量」的纹理图像，其像素颜色值对应三维法线向量的分量（通常映射到 [0, 1] 范围）：  
法线向量的三个分量（x, y, z）分别对应贴图的 RGB 通道：  

```
R = x *0.5 + 0.5
G = y* 0.5 + 0.5
B = z * 0.5 + 0.5
```

例如：纯蓝色（R=0.5, G=0.5, B=1.0）对应法线 (0, 0,1)，即垂直于表面向上（原始表面法线）。

假设切线空间中的法线向量为 (0.2, -0.3, 0.9)（表示表面略微向 X 轴正方向、Y 轴负方向倾斜，整体接近垂直表面）：  

```
R = (0.2 + 1) / 2 = 0.6（对应 RGB 中的红色分量为 0.6）
G = (-0.3 + 1) / 2 = 0.35（对应绿色分量为 0.35）
B = (0.9 + 1) / 2 = 0.95（对应蓝色分量为 0.95）
```

因此，该法线在纹理中显示为 (0.6, 0.35, 0.95) 的颜色（偏蓝绿色，因为 B 通道值较高，说明法线接近垂直表面）。

# 68

[shadplay](https://github.com/alphastrata/shadplay/)

# 67

[WGSL Function Reference](https://webgpufundamentals.org/webgpu/lessons/webgpu-wgsl-function-reference.html)

# 66

`cargo add avian2d`

```
use avian2d::{PhysicsPlugins, math::Vector, prelude::*};
use bevy::prelude::*;

pub fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(PhysicsPlugins::default())
        .add_plugins(PhysicsDebugPlugin)
        .insert_resource(Gravity(Vector::NEG_Y * 9.81 * 100.))
        .add_systems(Startup, setup)
        .add_systems(Update, log_envets)
        .run();
}

fn setup(mut commands: Commands) {
    // floor
    commands.spawn((
        RigidBody::Static,
        Collider::rectangle(500., 50.),
        Transform::from_xyz(0., -200., 0.),
    ));

    // ball
    commands.spawn((
        RigidBody::Dynamic,
        Collider::circle(100.),
        Transform::from_xyz(0., 200., 0.),
        CollisionEventsEnabled,
    ));

    commands.spawn(Camera2d);
}

fn log_envets(mut started: MessageReader<CollisionStart>, mut ended: MessageReader<CollisionEnd>) {
    for event in started.read() {
        println!("CollisionStart: {event:?}");
    }
    for event in ended.read() {
        println!("CollisionEnd: {event:?}");
    }
}
```

# 65

Events vs Messages

In previous versions of Bevy, the Event trait was used for both "observable events" (handled with Observer) and "buffered events" (handled with EventReader). This made some sense, as both concepts could be considered "events" in their own right. But they are also fundamentally very different things functionally (see this PR description for full rationale).

In Bevy 0.17, Event is now exclusively the name/trait for the concept of something that is "triggered" and "observed". Message is the name / trait for something that is "buffered": it is "written" via a MessageWriter and "read" via a MessageReader.

It is still possible to support both contexts by implementing both traits, but we expect that to be significantly less common than just choosing one.

# 64

Assets, Resource Usage

```
// assets.rs
#[derive(Debug, Resource)]
pub struct AudioAssets {
    pub laser_trigger: Handle<AudioSource>,
    pub ship_explosion: Handle<AudioSource>,
    pub ship_contact: Handle<AudioSource>,
    pub asteroid_explosion: Handle<AudioSource>,
}

pub struct AssetsPlugin;

impl Plugin for AssetsPlugin {
    fn build(&self, app: &mut App) {
        app.add_systems(OnEnter(AppState::Setup), setup);
    }
}

fn setup(mut commands: Commands, asset_server: Res<AssetServer>) {
}

fn setup(
    mut commands: Commands,
    asset_server: ResMut<AssetServer>,
    mut state: ResMut<NextState<GameState>>,
) {
    commands.insert_resource(AudioAssets {
        laser_trigger: asset_server.load("sfx_laser1.ogg"),
        ship_explosion: asset_server.load("Explosion_ship.ogg"),
        ship_contact: asset_server.load("Explosion.ogg"),
        asteroid_explosion: asset_server.load("Explosion.ogg"),
    });

    state.set(GameState::Menu);
}
```

```
// explosion.rs
fn catch_explosion_event(
    mut commands: Commands,
    mut event_reader: EventReader<SpawnExplosionEvent>,
    handles: Res<SpriteAssets>,
    audios: Res<AudioAssets>,
) {
    auidos.ship_explosion.clone()
}
```

# 63

`EventReader<KeyboardInput>` and `Res<ButtonInput<KeyCode>>`

```
/// Track keyboard inputs — useful for debugging or keybinding tools
fn log_keyboard_input(mut keyboard_events: EventReader<KeyboardInput>) {
    for event in keyboard_events.read() {
        println!(
            "Key pressed: {:?}, logical key: {:?}",
            event.key_code, event.logical_key
        );
    }
}
// or
fn jump_input_system(input: Res<ButtonInput<KeyCode>>) {
    if input.just_pressed(KeyCode::Space) {
        info!("Jump!");
    }
}
```

# 62

FileDragAndDrop

```
fn file_drag_and_drop_systems(mut event_reader: EventReader<FileDragAndDrop>) {
    for event in event_reader.read() {
        let file_path = match event {
            FileDragAndDrop::HoveredFile { window, path_buf } => Some(path_buf),
            FileDragAndDrop::DroppedFile { window, path_buf } => Some(path_buf),
            FileDragAndDrop::HoveredFileCanceled { window } => None,
        };

        if let Some(path) = file_path {
            dbg!(path);
        }
    }
}
```

# 61

[bevy game development](https://taintedcoders.com/)

# 60

Text has Node

```
#[derive(Component, Debug, Default, Clone, Deref, DerefMut, Reflect, PartialEq)]
#[reflect(Component, Default, Debug, PartialEq, Clone)]
#[require(Node, TextLayout, TextFont, TextColor, TextNodeFlags, ContentSize)]
pub struct Text(pub String);
```

# 59

-> impl Bundle

```
fn create_button() -> impl Bundle {
    (
        Button,
        Node {
            width: Val::Px(150.0),
            height: Val::Px(65.0),
            border: UiRect::all(Val::Px(5.0)),
            // horizontally center child text
            justify_content: JustifyContent::Center,
            // vertically center child text
            align_items: AlignItems::Center,
            ..default()
        },
        BorderColor(Color::BLACK),
        BorderRadius::MAX,
        BackgroundColor(Color::srgb(0.15, 0.15, 0.15)),
    )
}
```

# 58

Button has Interaction.

```
/// Marker struct for buttons
6#[derive(Component, Debug, Default, Clone, Copy, PartialEq, Eq, Reflect)]
7#[reflect(Component, Default, Debug, PartialEq, Clone)]
8#[require(Node, FocusPolicy::Block, Interaction)]
9pub struct Button;
```

# 57

```
#![allow(clippy::collapsible_if)]
```

# 56

```
Node{
    pub align_items: AlignItems, // single line, vertical
    pub justify_items: JustifyItems, // single line, horizontal
    pub align_self: AlignSelf, // work on child
    pub justify_self: JustifySelf, // work on child
    pub align_content: AlignContent, // mult line, vertical
    pub justify_content: JustifyContent, // mult line, horizontal
}
```

# 55

smooth follow

```
following.translation.smooth_nudge(&target.translation, decay_rate, delta_time);
```

# 54

despawn

```
commands.entity(trigger.target()).despawn() // also despawn the observer
```

# 53

- A type is `Send` if it is safe to send it to another thread.
- A type is `Sync` if it is safe to share between threads (T is Sync if and only if &T is Send).

# 52

```
App::new()
    .add_plugins(
        DefaultPlugins.set(WindowPlugin {
            primary_window: Some(Window {
                title: "I am a window!".into(),
                name: Some("bevy.app".into()),
                resolution: (500., 300.).into(),
                present_mode: bevy::window::PresentMode::AutoVsync,
                fit_canvas_to_parent: true,
                window_theme: Some(WindowTheme::Dark),
                enabled_buttons: EnabledButtons {
                    maximize: false,
                    ..default()
                },
                // visible: false,
                ..default()
            }),
            ..default()
        })
    );
```

# 51

replace .0

```
#[derive(Component, Dered, DerefMut)]
struct AnimationTimer(Timer);
```

# 50

ListNode

```
#[derive(Debug)]
pub struct ListNode {
    pub val: i32,
    pub next: Option<Box<ListNode>>,
}
impl ListNode {
    pub fn new(val: i32) -> Self {
        ListNode { val, next: None }
    }
}

pub fn main() {
    let mut node3 = ListNode::new(3);
    node3.next = None;
    let mut node2 = ListNode::new(2);
    node2.next = Some(Box::new(node3));
    let mut node1 = ListNode::new(1);
    node1.next = Some(Box::new(node2));
    let mut cur = Some(Box::new(node1));
    while let Some(n) = cur {
        println!("{:?}", &n);
        cur = n.next;
    }
}
```

# 49

error: Unknown binary 'rust-analyzer' in official toolchain 'stable-x86_64-unknown-linux-gnu'.

```
rustup component add rust-analyzer
```

# 48

[WebGPU Live Demo Editor](https://www.wgsl.dev/editor)

# 47

[bevy io for wgsl](https://github.com/bevyengine/bevy/blob/main/crates/bevy_pbr/src/render/mesh.wgsl)

# 46

[Shadertoy](https://www.shadertoy.com/)

# 45

[Option](https://doc.rust-lang.org/std/option/) => Some, None  
[Result](https://doc.rust-lang.org/std/result/index.html) => Ok, Err -> if/while let

# 44

Each bundle represents a static set of `Component` types. Currently, bundles can only contain one of each `Component`, and will panic once initialized if this is not met.

# 43

Handles act as abstract "references" to assets, whose data are stored in the `Assets<A>` resources, avoiding the need to store multiple copies of the same data.

# 42

When Ownership Moves (Transfers)

1. Assignment: `let x = y;` (where y is non-Copy type)
2. Function calls: Passing non-Copy values to functions
3. Returns: Returning values from functions
4. Collections: Inserting/removing from `Vec`, `HashMap`, etc.
5. Structs/Enums: Assigning to fields or variants
6. Pattern matching: Destructuring with `let` or `match`
7. Closures: Using move keyword or capturing values
8. Operators: Some overloaded operators (like `+` for Strings)
9. Loops: Using `into_iter()` explicitly or implicitly

# 41

[docs.rs](https://docs.rs)

# 40

trigger

```
fn attack_hits(trigger: Trigger<Attack>, name: Query<&Name>) {
    if let Ok(name) = name.get(trigger.target()) {
        info!("Attack hit {}", name);
    }
```

# 39

tests with panic

```
#[should_panic = "Rectangle width and height must be positive"]
```

# 38

generics and traits

```
impl<T: std::fmt::Display> ReportCard<T> {
    fn print(&self) -> String {
        format!(
            "{} ({}) - achieved a grade of {}",
            &self.student_name, &self.student_age, &self.grade,
        )
    }
}
```

# 37

`MeshPickingPlugin`, `observe`

# 36

unit struct is just a tag.  

# 35

parameters of function is also declaration.  
[mut](https://doc.rust-lang.org/std/keyword.mut.html)

# 34

deref coercion

# 33

The `+` operator can concatenate a `String` with `&str`

# 32

[rustlings](https://rustlings.rust-lang.org/)

# 31

[Blockchain in Rust](https://www.youtube.com/playlist?list=PLwnSaD6BDfXL0RiKT_5nOIdxTxZWpPtAv)

# 30

- `a : &T`: 引用不可变，且引用指向的内容不可修改。
- `a : &mut T`: 引用不可变，引用指向内容可以修改。
- `mut a: &T`: 引用可变（a可重新赋值），但引用指向内容不可修改。
- `mut a: &mut T` 引用可变，引用指向内容可以修改。

# 29

win install rust with china mirror

```
$ENV:RUSTUP_DIST_SERVER='https://mirrors.ustc.edu.cn/rust-static' 
$ENV:RUSTUP_UPDATE_ROOT='https://mirrors.ustc.edu.cn/rust-static/rustup'

.\rustup-init.exe
```

# 28

```
cargo build --timings
cargo tree -e feature -i <crate>
```

# 27

```
fn fill_vec(mut vec: Vec<i32>)
vec本身可变

fn fill_vec(vec: &mut Vec<i32>)
vec绑定内容可变

```

# 26

Blocking waiting for file lock on package cache

```
rm -rf ~/.cargo/.package-cache

lsof | grep .package-cache
kill -9 <PID>
```

# 25

[Hexagonal Grids](https://www.redblobgames.com/grids/hexagons/)

# 24

[Installing Linux dependencies](https://github.com/bevyengine/bevy/blob/main/docs/linux_dependencies.md)

# 23

[Making a Flappy Bird Game with Bevy & Rust in under 10 Minutes](https://www.youtube.com/watch?v=_C28kqin94c&ab_channel=BipedPotato)

# 23

[Unofficial Bevy Cheat Book](https://bevy-cheatbook.github.io/introduction.html)

# 22

[Leaving Rust gamedev after 3 years](https://loglog.games/blog/leaving-rust-gamedev/)

# 21

Option 代表可能为空可能有值的一种类型，本质上是一个枚举，有两种分别是 Some 和 None。Some 代表有值，None 则类似于 null，代表无值。

# 20

不可变引用可以重新分配，但是不能修改引用的值  
可变引用可以修改引用的值，但不能重新分配

# 19

When doing assignments (let x = y) or passing function arguments by value (foo(x)), the ownership of the resources is transferred. In Rust-speak, this is known as a move. After moving resources, the previous owner can no longer be used. This avoids creating dangling pointers.

# 18

[steamdb](https://steamdb.info)

# 17

`#![outer_attribute]` applies to the item immediately following it. Some examples of items are: a function, a module declaration, a constant, a structure, an enum.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
```

`#![inner_attribute]` applies to the enclosing item (typically a module or a crate). In other words, this attribute is interpreted as applying to the entire scope in which it's placed.

```rust
#![allow(unused_variables)]

fn main() {
    let x = 3; // This would normally warn about an unused variable.
}
```

# 17

Self和self区别  
Self是一个类型，用于引用当前的类型，而self是一个值，用于引用当前实例。
Self通常在trait和impl 块中使用，而self只能在方法定义中使用。
Self可以用于返回类型，类型参数等。而self用于方法参数列表中，表示当前实例的引用。

# 16

- **iter** - This borrows each element of the collection through each iteration. Thus leaving the collection untouched and available for reuse after the loop.
- **into_iter** - This consumes the collection so that on each iteration the exact data is provided. Once the collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop.
- **iter_mut** - This mutably borrows each element of the collection, allowing for the collection to be modified in place.

# 15

The From and Into traits are inherently linked, and this is actually part of its implementation. If you are able to convert type A from type B, then it should be easy to believe that we should be able to convert type B to type A.

# 14

Rust provides no implicit type conversion (coercion) between primitive types. But, explicit type conversion (casting) can be performed using the as keyword.

# 13

```
my-cool-project
├── godot
│   ├── project.godot
│   └── my-extension.gdextension
└── rust
    ├── Cargo.toml
    ├── src
    └── target
        └── debug
            └── (lib)?my_extension.(so|dll|dylib)
```

# 12

权限在引用生命周期结束时被返回

```rust
let mut x = 1; //x(r,w,o)
let y = &x; // x(r)
let z = *y; // x(r,w,o)
x += z; // x(r,w,o) z(r)
```

# 11

可变引用可以降级为只读引用

# 10

可变引用提供对数据“唯一”且""访问  
不移动数据的情况下，可访问  
可变引用 &mut  
相当于常量指针 cosnt * 不移动 可以修改

# 9

```rust
let mut v: Vec<i32> = vec![1,2,3]; //v(R,W,O)
let num: &i32 = &v[2]; // v(R) num(R,O) *num(R)
v.push(r); //error v is borrowed by num
println!("Third element is {*num}"); // immutable borrow
```

# 8

RW(mut)O

# 7

可变性和别名不可同时存在(Rust Avoids Simultaneous Aliasing and Mutation)

# 6

引用是没有所有权的指针

# 5

[godot-rust](https://colinwttt.github.io/godot-rust-book-chinese/index.html)

# 4

[Rust 程序语言设计](https://www.rustwiki.org.cn/zh-CN/book/title-page.html)

# 3

```
cargo new project_name
cd project_name
cargo build
cargo run
cargo check
```

# 2

[Godot 4 & Rust](https://www.youtube.com/watch?v=G9UDVtR5AYA&list=PLPC9niKFOaRqYsXAapTVR8pqJmQColixs&index=1&ab_channel=schr3da)

# 1

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup update
rustup self uninstall
```
