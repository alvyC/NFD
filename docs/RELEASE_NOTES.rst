.. _NFD Release Notes:

NFD Release Notes
=================

NFD version 0.3.0 (changes since version 0.2.0)
-----------------------------------------------

Release date: February 2, 2015


New features:
^^^^^^^^^^^^^

- **Build**

  + The code now requires C++11.  The minimum supported gcc version is 4.6, as earlier versions
    do not have proper support for C++11 features.

- **Faces**

  + Enable detection of WebSocket connection failures using ping/pong messages (:issue:`1903`)

  + In EthernetFace:

    * Avoid putting the NIC in promiscuous mode if possible (:issue:`1278`)

    * Report packets dropped by the kernel if debug is enabled (:issue:`2441`)

    * Integrate NDNLP fragmentation (:issue:`1209`)

- **Forwarding**

  + Strategy versioning (:issue:`1893`)

  + New Dead Nonce List table to supplement PIT for loop detection (:issue:`1953`)

  + Abstract retransmission suppression logic (:issue:`2377`)

  + New forwarding strategy for access router (:issue:`1999`)

- **Management**

  + Add config file-based strategy selection (:issue:`2053`)

    The sample config file now includes strategy selection for ``/``, ``/localhost``,
    ``/localhost/nfd``, and ``/ndn/broadcast`` namespaces as follows:

    ::

        tables
        {
          ...
          strategy_choice
          {
            /               /localhost/nfd/strategy/best-route
            /localhost      /localhost/nfd/strategy/broadcast
            /localhost/nfd  /localhost/nfd/strategy/best-route
            /ndn/broadcast  /localhost/nfd/strategy/broadcast
          }
        }

  + Implement Query Operation in FaceManager (:issue:`1993`)

  + FaceManager now responds with producer-generated NACK when query is invalid (:issue:`1993`)

  + Add functionality for automatic remote prefix registration (:issue:`2056`)

  + Only canonical FaceUri are allowed in faces/create commands (:issue:`1910`)

- **Tables**

  + StrategyInfoHost can now store multiple StrategyInfo of distinct types (:issue:`2240`)

  + Enable iteration over PIT and CS entries (:issue:`2339`)

  + Allow predicate to be specified in Measurements::findLongestPrefixMatch (:issue:`2314`)

  + Calculate the implicit digest of Data packets in CS only when necessary (:issue:`1706`)

- **Tools**

  + Publish ``/localhop/ndn-autoconf/routable-prefixes`` from ``ndn-autoconfig-server``
    (:issue:`1954`)

  + Display detailed NFD software verion in ``nfd-status-http-server`` and ``nfd-status``
    (:issue:`1916`)

  + ``nfdc`` now accepts FaceUri in all commands (:issue:`1995`)

  + Add daemon mode for ``ndn-autoconfig`` to re-run detection when connectivity changes
    (:issue:`2417`)

- **Core**

  + New scheduler::ScopedEventId class to automatically handle scheduled event lifetime
    (:issue:`2295`)

Updates and bug fixes:
^^^^^^^^^^^^^^^^^^^^^^

- **Documentation**

  + NFD Developer's guide has been updated to reflect changes in the codebase

  + Installation instruction updates

  + Update of config file instructions for disabling unix sockets (:issue:`2190`)

- **Core**

  + Use implementations moved to ndn-cxx library

     + Use Signal from ndn-cxx (:issue:`2272`, :issue:`2300`)

     + use ethernet::Address from ndn-cxx (:issue:`2142`)

     + Use MAX_NDN_PACKET_SIZE constant from ndn-cxx (:issue:`2099`)

     + Use DEFAULT_INTEREST_LIFETIME from ndn-cxx (:issue:`2202`)

     + Use FaceUri from ndn-cxx (:issue:`2143`)

     + Use DummyClientFace from ndn-cxx (:issue:`2186`)

     + Use ndn::dns from ndn-cxx (:issue:`2207`)

  + Move Network class implementation from ``tools/`` to ``core/``

  + Ignore non-Ethernet ``AF_LINK`` addresses when enumerating NICs on OSX and other BSD systems

  + Fix bug on not properly setting FreshnessPeriod inside SegmentPublisher (:issue:`2438`)

- **Faces**

  + Fix spurious assertion failure in StreamFace (:issue:`1856`)

  + Update websocketpp submodule (:issue:`1903`)

  + Replace FaceFlags with individual fields (:issue:`1992`)

  + Drop WebSocket message if the size is larger than maximum NDN packet size (:issue:`2081`)

  + Make EthernetFace more robust against errors (:issue:`1984`)

  + Prevent potential infinite loop in TcpFactory and UdpFactory (:issue:`2292`)

  + Prevent crashes when attempting to create a UdpFace over a half-working connection
    (:issue:`2311`)

  + Support MTU larger than 1500 in EthernetFace (for jumbo frames) (:issue:`2305`)

  + Re-enable EthernetFace on OS X platform with boost >=1.57.0 (:issue:`1922`)

  + Fix ``ioctl()`` calls on platforms where libpcap uses ``/dev/bpf*`` (:issue:`2327`)

  + Fix overhead estimation in NDNLP slicer (:issue:`2317`)

  + Replace usage of deprecated EventEmitter with Signal in Face abstractions (:issue:`2300`)

  + Fix NDNLP PartialMessage cleanup scheduling (:issue:`2414`)

  + Remove unnecessary use of DNS resolver in (Udp|Tcp|WebSocket)Factory (:issue:`2422`)

- **Forwarding**

  + Updates related to NccStrategy

    * Fix to prevent remembering of suboptimal upstreams (:issue:`1961`)

    * Optimizing FwNccStrategy/FavorRespondingUpstream test case (:issue:`2037`)

    * Proper detection for new PIT entry (:issue:`1971`)

    * Use UnitTestTimeFixture in NCC test case (:issue:`2163`)

    * Fix loop back to sole downstream (:issue:`1998`)

  + Updates related to BestRoute strategy

    + Redesign best-route v2 strategy test case (:issue:`2126`)

    + Fix clang compilation error in best-route v2 test case (:issue:`2179`)

    + Use UnitTestClock in BestRouteStrategy2 test (:issue:`2160`)

  + Allow strategies limited access to FaceTable (:issue:`2272`)

- **Tables**

  + Ensure that eviction of unsolicited Data is done in FIFO order (:issue:`2043`)

  + Simplify table implementations with C++11 features (:issue:`2100`)

  + Fix issue with Fib::removeNextHopFromAllEntries invalidating NameTree iterator
    (:issue:`2177`)

  + Replace deprecated EventEmitter with Signal in FaceTable (:issue:`2272`)

  + Refactored implementation of ContentStore based on std::set (:issue:`2254`)

- **Management**

  + Allow omitted FaceId in faces/create command (:issue:`2031`)

  + Avoid deprecated ``ndn::nfd::Controller(Face&)`` constructor (:issue:`2039`)

  + Enable check of command length before accessing verb (:issue:`2151`)

  + Rename FaceEntry to Route (:issue:`2159`)

  + Insert RIB command prefixes into RIB (:issue:`2312`)

- **Tools**

  + Display face attribute fields instead of FaceFlags in ``nfd-status`` and
    ``nfd-status-http-server`` output (:issue:`1991`)

  + Fix ``nfd-status-http-server`` hanging when nfd-status output is >64k (:issue:`2121`)

  + Ensure that ``ndn-autoconfig`` canonizes FaceUri before sending commands to NFD
    (:issue:`2387`)

  + Refactored ndn-autoconfig implementation (:issue:`2421`)

  + ndn-autoconfig will now register also ``/localhop/nfd`` prefix towards the hub (:issue:`2416`)

- **Tests**

  + Use UnitTestClock in Forwarder persistent loop test case (:issue:`2162`)

  + Use LimitedIo in FwForwarder/SimpleExchange test case (:issue:`2161`)

- **Build**

  + Fix build error with python3 (:issue:`1302`)

  + Embed CI build and test running script

  + Properly disable assertions in release builds (:issue:`2139`)

  + Embed setting of ``PKG_CONFIG_PATH`` variable to commonly used values (:issue:`2178`)

  + Add conditional compilation for NetworkInterface and PrivilegeHelper

  + Support tools with multiple translation units (:issue:`2344`)

Removals
^^^^^^^^

- Remove ``listen`` option from unix channel configuration (:issue:`2188`)

- Remove usage of deprecated ``MetaInfo::TYPE_*`` constants (:issue:`2128`)

- Eliminate MapValueIterator in favor of ``boost::adaptors::map_values``

****************************************************************************

NFD version 0.2.0 (changes since version 0.1.0)
-----------------------------------------------

Release date: August 25, 2014

- **Documentation**

  + `"NFD Developer's Guide" by NFD authors
    <http://named-data.net/wp-content/uploads/2014/07/NFD-developer-guide.pdf>`_ that
    explains NFD's internals including the overall design, major modules, their
    implementation, and their interactions

  + New detailed instructions on how to enable auto-start of NFD using OSX ``launchd``
    and Ubuntu's ``upstart`` (see `contrib/ folder
    <https://github.com/named-data/NFD/tree/master/contrib>`_)

- **Core**

  + Add support for temporary privilege drop and elevation (:issue:`1370`)

  + Add support to reinitialize multicast Faces and (partially) reload config file
    (:issue:`1584`)

  + Randomization routines are now uniform across all NFD modules (:issue:`1369`)

  + Enable use of new NDN naming conventions (:issue:`1837` and :issue:`1838`)

- **Faces**

  + `WebSocket <http://tools.ietf.org/html/rfc6455>`_ Face support (:issue:`1468`)

  + Fix Ethernet Face support on Linux with ``libpcap`` version >=1.5.0 (:issue:`1511`)

  + Fix to recognize IPv4-mapped IPv6 addresses in ``FaceUri`` (:issue:`1635`)

  + Fix to avoid multiple onFail events (:issue:`1497`)

  + Fix broken support of multicast UDP Faces on OSX (:issue:`1668`)

  + On Linux, path MTU discovery on unicast UDPv4 faces is now disabled (:issue:`1651`)

  + Added link layer byte counts in FaceCounters (:issue:`1729`)

  + Face IDs 0-255 are now reserved for internal NFD use (:issue:`1620`)

  + Serialized StreamFace::send(Interest|Data) operations using queue (:issue:`1777`)

- **Forwarding**

  + Outgoing Interest pipeline now allows strategies to request a fresh ``Nonce`` (e.g., when
    the strategy needs to re-express the Interest) (:issue:`1596`)

  + Fix in the incoming Data pipeline to avoid sending packets to the incoming Face
    (:issue:`1556`)

  + New ``RttEstimator`` class that implements the Mean-Deviation RTT estimator to be used in
    forwarding strategies

  + Fix memory leak caused by not removing PIT entry when Interest matches CS (:issue:`1882`)

  + Fix spurious assertion in NCC strategy (:issue:`1853`)

- **Tables**

  + Fix in ContentStore to properly adjust internal structure when ``Cs::setLimit`` is called
    (:issue:`1646`)

  + New option in configuration file to set an upper bound on ContentStore size (:issue:`1623`)

  + Fix to prevent infinite lifetime of Measurement entries (:issue:`1665`)

  + Introducing capacity limit in PIT NonceList (:issue:`1770`)

  + Fix memory leak in NameTree (:issue:`1803`)

  + Fix segfault during Fib::removeNextHopFromAllEntries (:issue:`1816`)

- **Management**

  + RibManager now fully support ``CHILD_INHERIT`` and ``CAPTURE`` flags (:issue:`1325`)

  + Fix in ``FaceManager`` to respond with canonical form of Face URI for Face creation command
    (:issue:`1619`)

  + Fix to prevent creation of duplicate TCP/UDP Faces due to async calls (:issue:`1680`)

  + Fix to properly handle optional ExpirationPeriod in RibRegister command (:issue:`1772`)

  + Added functionality of publishing RIB status (RIB dataset) by RibManager (:issue:`1662`)

  + Fix issue of not properly canceling route expiration during processing of ``unregister``
    command (:issue:`1902`)

  + Enable periodic clean up of route entries that refer to non-existing faces (:issue:`1875`)

- **Tools**

  + Extended functionality of ``nfd-status``

     * ``-x`` to output in XML format, see :ref:`nfd-status xml schema`
     * ``-c`` to retrieve channel status information (enabled by default)
     * ``-s`` to retrieve configured strategy choice for NDN namespaces (enabled by default)
     * Face status now includes reporting of Face flags (``local`` and ``on-demand``)
     * On-demand UDP Faces now report remaining lifetime (``expirationPeriod``)
     * ``-r`` to retrieve RIB information

  + Improved ``nfd-status-http-server``

     * HTTP server now presents status as XSL-formatted XML page
     * XML dataset and formatted page now include certificate name of the corresponding NFD
       (:issue:`1807`)

  + Several fixes in ``ndn-autoconfig`` tool (:issue:`1595`)

  + Extended options in ``nfdc``:

    * ``-e`` to set expiration time for registered routes
    * ``-o`` to specify origin for registration and unregistration commands

  + Enable ``all-faces-prefix'' option in ``nfd-autoreg`` to register prefix for all face
    (on-demand and non-on-demand) (:issue:`1861`)

  + Enable processing auto-registration in ``nfd-autoreg`` for faces that existed prior to
    start of the tool (:issue:`1863`)

- **Build**

  + Enable support of precompiled headers for clang and gcc to speed up compilation

- `Other small fixes and extensions
  <https://github.com/named-data/NFD/compare/NFD-0.1.0...master>`_

****************************************************************************

NFD version 0.1.0
-----------------

Release date: May 7, 2014

This is an incomplete list of features that are implemented in NFD version 0.1.0.

- **Packet Format**

  + `NDN-TLV <http://named-data.net/doc/ndn-tlv/>`_
  + LocalControlHeader, to allow apps to set outgoing face and learn incoming face.

- **Faces**

  + Unix stream socket
  + UDP unicast
  + UDP multicast
  + TCP
  + Ethernet, currently without fragmentation.

    .. note::
         Ethernet support will not work properly on Linux kernels with TPACKET_V3 flexible
         buffer implementation (>= 3.2.0) and libpcap >= 1.5.0 (e.g., Ubuntu Linux 14.04).
         Refer to `Issue 1551 <http://redmine.named-data.net/issues/1511>`_ for more
         detail and implementation progress.

- **Management**

  + Use of signed Interests as commands, with authentication and authorization.
  + Face management
  + FIB management
  + Per-namespace strategy selection
  + NFD status publishing
  + Notification to authorized apps of internal events, including Face creation and destruction.

- **Tables and forwarding pipelines** support most Interest/Data processing, including
  selectors.

- **RIB Management** that runs as a separate process, ``nrd``.  It supports basic prefix
  registration by applications, but no flags yet.

- **Strategies**

  + ``broadcast``
  + ``best-route``
  + ``ncc``: based on ccnx 0.7 for experimentation
  + ``client-control``: authorized application can directly control Interest forwarding

- **Name-based scoping**

  + ``/localhost``: communication only within localhost using "local" Faces
    (UnixStreamFace, LocalTcpFace).  NFD will strictly enforce this scope for Interests
    and Data packets
  + ``/localhop``: one-hop communication (e.g., if at least one incoming or outgoing Face
    in PIT entry is non-local, the Interest cannot be forwarded to any non-local Face)

- **Support configuration file**, which is in the Boost INFO format.

- **Applications**

  + Tools to discover hubs on NDN testbed.
  + peek/poke and traffic generators for testing and debugging.
  + ``nfdc``, a command-line tool to configure NFD.
  + ``nfd-status``, a command-line tool to query NFD status.
  + ``nfd-status-http-server``, which reads the NFD status and publishes over HTTP.


Planned Functions and Features for Next Releases
------------------------------------------------

- NACK
    A packet sent back by a producer or a router to signal the unavailability of a requested
    Data packet. The protocol specification for NACK is in progress.

- New strategies
    Additional strategies, including self-learning that populates the FIB by observing
    Interest and Data exchange.

- Hop-by-hop Interest limit mechanism
    For congestion control

- Face enhancements
    Add fragmentation support for Ethernet face, may add support for new types such as
    WiFi direct and WebSockets.

- Tables
    Experiment and evaluate different data structures and algorithms.

- RIB management
    Move to more scalable data structures and support all flags in prefix registrations.

- Tunnel management
    For hub nodes to authenticate incoming tunnel requests and maintain the tunnels.

- Extensible name-based scoping
    Configurable organization-based scoping
