Visibility
==========

The information _leaked_ to the consumers about the requirements **depends on the visibility**
declared in that transitive dependency, but it also depends on the **type on the node** in the
middle.

TBD: Does it depends on the type of the consumer too? It shouldn't, the consumer receive the data and
it is up to itself to consume whatever it needs.


 * **assets** in the middle: this is a package manager, probably assets should be always a leaf in the
   graph, or, as much, they can depend on other assets or build tools (tools to generate these assets
   using a dependent _assets_ package):

    |                 | Headers | Headers | Headers | Linking | Linking | Linking | Runtime | Runtime | Runtime |
    |-----------------|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
    |                 |  Iface  |  Public | Private |  Iface  |  Public | Private |  Iface  |  Public | Private |
    | assets          |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    -    |
    | tool-build      |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |
    | plugin-build    |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |


 * **library-header** in the middle: a _library-header_ cannot declare anything as _private_ or _public_,
   everything is _interface_.

    |                 | Headers | Linking | Runtime |
    |-----------------|:-------:|:-------:|:-------:|
    |                 |  Iface  |  Iface  |  Iface  |
    | assets          |    o    |    o    |    +    |
    | library-header  |    +    |    o    |    o    |
    | library-static  |    +    |    +    |    o    |
    | library-dynamic |    +    |    +    |    +    |
    | tool-host       |    o    |    o    |    +    |
    | tool-build      |    o    |    o    |    e    |
    | plugin-host     |    o    |    o    |    +    |
    | plugin-build    |    o    |    o    |    e    |


 * **library-static** in the middle:

    |                 | Headers | Headers | Headers | Linking | Linking | Linking | Runtime | Runtime | Runtime |
    |-----------------|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
    |                 |  Iface  |  Public | Private |  Iface  |  Public | Private |  Iface  |  Public | Private |
    | assets          |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    -    |
    | library-header  |    +    |    +    |    -    |    o    |    o    |    o    |    o    |    o    |    o    |
    | library-static  |    +    |    +    |    -    |    +    |    +    |    +    |    o    |    o    |    o    |
    | library-dynamic |    +    |    +    |    -    |    +    |    +    |    +    |    +    |    +    |    +    |
    | tool-host       |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    e    |
    | tool-build      |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |
    | plugin-host     |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    e    |
    | plugin-build    |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |


 * **library-dynamic** in the middle:

    |                 | Headers | Headers | Headers | Linking | Linking | Linking | Runtime | Runtime | Runtime |
    |-----------------|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
    |                 |  Iface  |  Public | Private |  Iface  |  Public | Private |  Iface  |  Public | Private |
    | assets          |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    -    |
    | library-header  |    +    |    +    |    -    |    o    |    o    |    o    |    o    |    o    |    o    |
    | library-static  |    +    |    +    |    -    |    +    |    +    |    -    |    o    |    o    |    o    |
    | library-dynamic |    +    |    +    |    -    |    +    |    +    |    -    |    +    |    +    |    +    |
    | tool-host       |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    e    |
    | tool-build      |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |
    | plugin-host     |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    e    |
    | plugin-build    |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |


 * **tool-host** in the middle, same as **tool-build**, **plugin-host** or **plugin-build**: only propagates
   runtime from the artifacts in _host_ context.

    |                 | Headers | Headers | Headers | Linking | Linking | Linking | Runtime | Runtime | Runtime |
    |-----------------|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
    |                 |  Iface  |  Public | Private |  Iface  |  Public | Private |  Iface  |  Public | Private |
    | assets          |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    -    |
    | library-header  |    -    |    -    |    -    |    o    |    o    |    o    |    o    |    o    |    o    |
    | library-static  |    -    |    -    |    -    |    -    |    -    |    -    |    o    |    o    |    o    |
    | library-dynamic |    -    |    -    |    -    |    -    |    -    |    -    |    +    |    +    |    +    |
    | tool-host       |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    e    |
    | tool-build      |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |
    | plugin-host     |    o    |    o    |    o    |    o    |    o    |    o    |    +    |    +    |    e    |
    | plugin-build    |    o    |    o    |    o    |    o    |    o    |    o    |    e    |    e    |    e    |
