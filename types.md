Types
=====

Given the different types of artifacts, not all of them have all the information
available and, hence, not all of the _can_ propagate the same information. But
here we are interested in a different approach, we want to know if the typology
of the artifact (and not the data it publish) influences what should be propagated
to its consumers.

From the point of view of the direct consumer, information taken from the
requirements:

|                 | Headers | Linking | Runtime | Package-ID |
|-----------------|---------|---------|---------|------------|
| assets          |    o    |    o    |    e    |      +     |
| library-header  |    +    |    o    |    e    |      +     |
| library-static  |    +    |    +    |    e    |      +     |
| library-dynamic |    +    |    +    |    e    |      +     |
| tool-host       |    o    |    o    |    e    |      +     |
| tool-build      |    o    |    o    |    +    |      -     |
| plugin-host     |    o    |    o    |    e    |      +     |
| plugin-build    |    o    |    o    |    +    |      -     |


 * **runtime** information doesn't make sense unless it is a `build_requires`
   that will run associated to this node in the `build` step.
 * assets-Headers: a library might require external assets to be generated, not
   the assets that will be consumed by the application, but assets that the library
   will embed inside.
 * **visibility** is not too important here: _interface_ just means that this
   information is not consumed, while _public_ and _private_ means it will
   be consumed. So, nothing from interfacce, and everything from public or private.