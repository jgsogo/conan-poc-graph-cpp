All information will be encoded in a **graph** and **every node
should only know about its neighbours**, all the information
about the transitive requirements should be provided by the
node in the middle (it can override some behaviors like the
package-id-mode used to consume the dependencies).

``[Not sure]`` every node should provide two sets of
information: the one related to itself and the one propagated
from the transitive requirements (aggregating all the
transitive requirements).
