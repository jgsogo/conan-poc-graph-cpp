Examples
========

Trying to investigate/demonstrate the information required to consume different
types of artifacts from others and the transitive information that has to be
propagated downstream.

Information to be taken into account:
 * **Headers**: header files and everything needed to link/use a library at the time
   of building a dependent one. It includes header files and preprocessor definitions.
 * **Linking**: everything needed to link against another library.
 * **Runtime**: everything needed during runtime, to execute/use the artifact in the
   final product.
 * **Package-ID**: information very specific to Conan.


Types
-----

Different types of artifacts:
 * assets: intended to be used only by the final application/product.
 * library-header: header-only library.
 * library-static: static library.
 * library-dynamic: dynamically linked library.
 * tool: artifact containing an executable (or plugins for a tool).
 * plugin: dynamic library to be consumed as a plugin.

About types, the type of a node could be encoded in the node itself, but
it is also a property of the relation, the same node can be consumed as
different types.
 * Information that belongs to the node: header, static, dynamic; this
   information is typically encoded in the package-id.
 * Information that belongs to the relation: library, tool, plugin; only
   the consumer can decide how it will use a requirement.
Both coordinates have to be coherent. Who raises on this?


Visibility
----------

Think about three different **visibility** relations:
 * interface: current artifact doesn't use it, but its consumers will.
 * public: current artifact uses it and it is exposed to the consumers.
 * private: current artifact uses it and its consumers won't


Contexts
--------

Different **contexts**:
 * `host`: dependencies should use the same settings as the one requiring it.
 * `other`: usually it would `build`, but it can be any other.


No-topological
--------------

On top of all of this, we need to support **overrides** and **default_option** propagation
upstream, and we need to resolve conflicts.

---

Legend for the tables in these files:
 * `+` information is needed
 * `-` information is not needed (and it shouldn't arrive)
 * `o` producer doesn't provide this information
 * `e` this situation doesn't make sense
