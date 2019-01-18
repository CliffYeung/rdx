```

Library:
    MKRdxPat.hpp

Multi-key Radix PATRICIA Fast Search - OO C++ - A header only class

    - The MKRdxPat class allows the allocation of a fixed sized contiguous data store that holds data nodes
      of an arbitrary structure that may be accessed with any number of keys of any size with the PATRICIA
      (Practical Algorithm To Retrieve Information Coded In Alphanumeric)(1,2,3) fast search algorithm.
      MKRdxPat is particularly suited to applications that require complex data structures be contiguously
      stored and accessed with an algorithm of known fast character with any of several possible keys,
      either singly of several simultaneously.  MKRdxPat is unique, as far as I know, in allowing search
      with multiple keys of any length.  If the application only requires a single key, this is OK; just
      set num_keys to 1 in the constructor call.  See below for the full documentation.

    - 16 cut/paste(MKRdxPat_test.cpp) verification tests that show code usage examples

    - no library dependencies(except standard header files)

    - DESIGN GOALS:
          1. Flexibility
          2. Originality
          3. Verifiability
          4. Testing
          5. Performance

    - see article in Embedded Systems Programming, Mar. 1997, "Highly dynamic Radix fast search
      algorithm with easy sort/deletion"(included)

    - see article in Embedded Systems Design, Jun. 2007, "Using multi-key radix PATRICIA fast
      search"(included)

    - performance measurement executable
          e.g.
              PERFORMANCE TEST: Do repeated rdx->insert()(fill trie) / rdx->remove()(empty trie)
                                monatonic keys
              Model name:            AMD Athlon(tm) 64 X2 Dual Core Processor 4000+
              CPU MHz:               2109.535

              max_rdx_nodes = 2,000,000
              num_keys = 3
              max_key_bytes = 16

              trie size = 558,000,327b

              seconds = 1131.294222  total inserts/removes = 400,000,000

    - see cut/paste Examples and API sections below

    - a rdx-compile.tar file is provided with a non-header compiled(clang, clang++, gcc, g++) version

    - this file is the entire documentation needed to use the library

    - Author: Richard Hogaboom, richard.hogaboom@gmail.com

    - ACKNOWLEDGMENT
        Pavel Odintsov(pavel.odintsov@gmail.com)

        Pavel provided the file of BGP routing prefixes from one of his BGP speakers
        for the MKRdxPat_perf_bgp performance test program

Usage:

    1. cd rdx/cpp

    2. a. the library is not dependent on any other libraries.
       b. the MKRdxPat_test.cpp executable has an extensive set of examples that can easily
          be cut and paste into your application.
       c. the MKRdxPat_perf.cpp executable measures performance.  the comments at the
          beginning of the file gives details.
       d. the MKRdxPat_perf_bgp.cpp executable measures performance with real BGP data.
          the comments at the beginning of the file gives details.

    3. adjust $CC in ./MKRdxPat.mk to use clang++ or g++

    4. do "./MKRdxPat.mk" to build and run the functional tests and compile the performance
       executables.

    5. do "./MKRdxPat.mk clean" to clean the directory

    6. vi MKRdxPat_perf.cpp and examine the comments at the beginning of the file to learn
       how to run the performance test.  to modify the performance tests to better fit your
       application edit MKRdxPat_perf.cpp and install your application data in
       'struct app_data{}' and set the MKRdxPat.hpp class constructor arguments:

       //
       // set these application specific data
       //
       // ================================================

       // MKRdxPat.hpp class constructor arguments
       const int max_rdx_nodes = 200000;
       const int num_keys      = 2;
       const int max_key_bytes = 16;

       // application data of type app_data defined here
       struct app_data
       {
           int i;
       };

       // ================================================


       do "./MKRdxPat.mk" to build

       do "./MKRdxPat_perf [options]" to run performance tests with
       your application specific data

    7. vi MKRdxPat_perf_bgp.cpp and examine the comments at the beginning of the file to learn
       how to run the BGP performance test.  to modify the BGP performance test to better fit
       your application edit MKRdxPat_perf_bgp.cpp and install your application data in
       'struct app_data{}'.  unpack BGP_routing_table_3peersnap_04102016_as198068.xz:

       //
       // change only struct app_data {}; if desired and nothing else
       //
       // ================================================

       // MKRdxPat.hpp class constructor arguments
       const int max_rdx_nodes = BGP_SIZE;  // fixed - routing table entries
       const int num_keys      = 1;  // fixed - one key(prefix) in routing table

       // store keys in trie as:\n"
       //     1 - d(3).d(3).d(3).d(3)/n(2) ascii(up to 18 bytes)
       //     2 - d(3)d(3)d(3)d(3)n(2) ascii(up to 14 bytes)
       //     3 - (32 bit uint)(4)n(1) binary(5 bytes)
       const int max_key_bytes = MAX_PREFIX_SIZE;

       // application data of type app_data defined here
       struct app_data
       {
           int i;
       };

       // ================================================

       do "./MKRdxPat.mk" to build

       do "./MKRdxPat_perf_bgp [options]" to run bgp performance tests with
       your application specific data

Files:

    BGP_routing_table_3peersnap_04102016_as198068.xz - compressed BGP data
    MKRdxPat.hpp                                     - header only C++ implementation
    MKRdxPat.mk                                      - build script, compiles/executes test code
    MKRdxPat_perf_bgp.cpp                            - BGP test code
    MKRdxPat_perf.cpp                                - performance test code
    MKRdxPat_test.cpp                                - test code
    rdx-compile.tar                                  - non-header compiled(clang, clang++, gcc, g++) version
    rdx.doc                                          - documentation for library

Examples:

    All examples are from the test code in MKRdxPat_test.cpp.

    NOTES:
        1. all tests are independent.  any test may be deleted; it will affect no other test
        2. try the tests with different values for MAX_RDX_NODES, NUM_KEYS and MAX_KEY_BYTES
        3. each test outputs to it's own file of filename format: MKRdxPat.TESTn.results
        4. use the './MKRdxPat.mk' script to compile and execute the tests
        5. use './MKRdxPat.mk clean' to clean the directory

    From TEST 1
        int return_code;

        // application data of type app_data defined here
        struct app_data
        {
            int id;
        };

        // maximum number of data nodes stored in rdx trie
        const int MAX_RDX_NODES = 4;

        // number of rdx search keys
        const int NUM_KEYS = 3;

        // number of bytes in each key(s)
        const int MAX_KEY_BYTES = 4;

        ofstream os;
        os.open("MKRdxPat.TEST0.results");

        os << "\n"
              "TEST 0: Print rdx trie before inserting any data nodes\n"
              "        Expected Results:\n"
              "           a. Only the impossible pre-allocated root node should be printed since\n"
              "              no other nodes have been inserted\n\n";

        os << "MAX_RDX_NODES = " << MAX_RDX_NODES << "\n"
              "NUM_KEYS = " << NUM_KEYS << "\n"
              "MAX_KEY_BYTES = " << MAX_KEY_BYTES << "\n\n";

        MKRdxPat<app_data> *rdx = new MKRdxPat<app_data>(MAX_RDX_NODES, NUM_KEYS, MAX_KEY_BYTES);

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n";
        os << "rdx - Bytes allocated = " << rdx->bsize() << "\n\n";


        os << "a. Only the impossible pre-allocated root node should be printed since\n";
        os << "   no other nodes have been inserted\n\n";
        return_code = rdx->print(NULL, os);

        os << "return_code = rdx->print(NULL, os); return_code = " << return_code << "\n\n";

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n";

        delete rdx;

        os.close();

    From TEST 2
        int return_code;

        // application data of type app_data defined here
        struct app_data
        {
            int id;
            double d;
        };

        app_data *app_datap;

        // maximum number of data nodes stored in rdx trie
        const int MAX_RDX_NODES = 1;

        // number of rdx search keys
        const int NUM_KEYS = 3;

        // number of bytes in each key(s)
        const int MAX_KEY_BYTES = 4;

        // MAX_RDX_NODES nodes of NUM_KEYS keys of MAX_KEY_BYTES bytes - set all key booleans to 1
        unsigned char rdx_key[MAX_RDX_NODES][NUM_KEYS][1+MAX_KEY_BYTES];

        memset(rdx_key, 0, MAX_RDX_NODES * NUM_KEYS * (1+MAX_KEY_BYTES));
        for ( int n = 0, sum = 0 ; n < MAX_RDX_NODES ; n++ )
        {
            for ( int k = 0 ; k < NUM_KEYS ; k++ )
            {
                rdx_key[n][k][0] = 1;  // set key boolean to 1
                rdx_key[n][k][MAX_KEY_BYTES] = sum++;
            }
        }

        ofstream os;
        os.open("MKRdxPat.TEST1.results");

        os << "\n"
              "TEST 1: Insert one data node in rdx trie, then insert node with same keys again\n"
              "        Expected Results:\n"
              "           a. One node insertion with return code 0\n"
              "           b. Total nodes allocated(not including root node) 1\n"
              "           c. No verification error\n"
              "           d. Print entire trie - allocated node and root node\n"
              "           e. Same keys node insertion should return code 1\n"
              "           f. Different keys node insertion into full rdx should return code 2\n"
              "           g. Same keys node insertion with a key boolean set to 0 should return code 3\n\n";

        os << "MAX_RDX_NODES = " << MAX_RDX_NODES << "\n"
              "NUM_KEYS = " << NUM_KEYS << "\n"
              "MAX_KEY_BYTES = " << MAX_KEY_BYTES << "\n\n";

        MKRdxPat<app_data> *rdx = new MKRdxPat<app_data>(MAX_RDX_NODES, NUM_KEYS, MAX_KEY_BYTES);

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n";
        os << "rdx - Bytes allocated = " << rdx->bsize() << "\n\n";


        os << "a. One node insertion with return code 0\n";
        print_key((unsigned char *)rdx_key[0], os, NUM_KEYS, MAX_KEY_BYTES);
        return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap);

        os << "return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap); return_code = " << return_code << "\n\n\n";


        os << "b. Total nodes allocated(not including root node) 1\n";
        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n";

        app_datap->id = 20;
        os << "set app_datap->id = 20\n";
        os << "get app_datap->id = " << app_datap->id << "\n";
        app_datap->d = 2.0;
        os << "set app_datap->d = 2.0\n";
        os << "get app_datap->d = " << app_datap->d << "\n\n";

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n\n";


        os << "c. No verification error\n";
        return_code = rdx->verify(ERR_CODE_PRINT, os);

        os << "return_code = rdx->verify(ERR_CODE_PRINT, os); verify success(0) or fail(!0) -> return_code = " << return_code << "\n\n\n";


        os << "d. Print entire trie - allocated node and root node\n";
        return_code = rdx->print(NULL, os);

        os << "return_code = rdx->print(NULL, os); return_code = " << return_code << "\n\n";

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n\n";


        os << "e. Same keys node insertion should return code 1\n";
        print_key((unsigned char *)rdx_key[0], os, NUM_KEYS, MAX_KEY_BYTES);
        return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap);

        os << "return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap); return_code = " << return_code << "\n\n";

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n\n";


        os << "f. Different keys node insertion into full rdx should return code 2\n";
        // set each key to a unique value not used before - this will try to insert into a full rdx and set return_code = 2
        // - if any key is the same as a previously inserted node then return_code = 1 will be returned
        rdx_key[0][0][1] = 1;  // set first key(key index 0) first key byte to different from 0
        rdx_key[0][1][1] = 1;  // set second key(key index 1) first key byte to different from 0
        rdx_key[0][2][1] = 1;  // set third key(key index 2) first key byte to different from 0
        print_key((unsigned char *)rdx_key[0], os, NUM_KEYS, MAX_KEY_BYTES);
        return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap);

        os << "return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap); return_code = " << return_code << "\n\n";

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n\n";


        os << "g. Same keys node insertion with a key boolean set to 0 should return code 3\n";
        rdx_key[0][1][0] = 0;  // set second key(key index 1) boolean to 0
        print_key((unsigned char *)rdx_key[0], os, NUM_KEYS, MAX_KEY_BYTES);
        return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap);

        os << "return_code = rdx->insert((unsigned char *)rdx_key[0], &app_datap); return_code = " << return_code << "\n\n";

        os << "rdx - Nodes allocated = " << rdx->alloc_nodes() << "\n\n";

        delete rdx;

        os.close();

API:

/*
 *======================================================================================================================
 *
 * File Names:
 *     MKRdxPat.hpp
 *
 *======================================================================================================================
 *
 * Member Functions:
 *
 *     struct app_data
 *     {
 *         int data;  // user specified application data structure
 *     };
 *
 *     MKRdxPat<app_data>
 *         (
 *             int MAX_RDX_NODES,
 *             int NUM_KEYS,
 *             int MAX_KEY_BYTES
 *         )
 *         e.g. MKRdxPat<app_data> *rdx = new MKRdxPat<app_data>(512, 3, 4);
 *
 *
 *     ~MKRdxPat()
 *         e.g. delete rdx;
 *
 *
 *     struct app_data
 *     {
 *         int data;  // user specified application data structure
 *     };
 *
 *         int
 *     insert
 *         (
 *             const unsigned char *key,  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
 *             app_data **app_datapp
 *         )
 *         e.g. int return_code = rdx->insert((unsigned char *)key, &app_datap);
 *
 *
 *     struct app_data
 *     {
 *         int data;  // user specified application data structure
 *     };
 *
 *         app_data *
 *     search
 *         (
 *             const unsigned char *key  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
 *         )
 *         e.g. app_data *app_datap = rdx->search((unsigned char *)key);
 *
 *
 *     struct app_data
 *     {
 *         int data;  // user specified application data structure
 *     };
 *
 *         app_data *
 *     remove
 *         (
 *             const unsigned char *key  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
 *         )
 *         e.g. app_data *app_datap = rdx->remove((unsigned char *)key);
 *
 *
 *         int
 *     sort
 *         (
 *             app_data ***app_datappp,
 *             const int k
 *         )
 *         e.g. int return_code = rdx->sort(&app_datapp, k);
 *
 *
 *         int
 *     keys
 *         (
 *             unsigned char *key  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
 *         )
 *         e.g. int return_code = rdx->keys((unsigned char *)key);
 *
 *
 *         int
 *     alloc_nodes
 *         (
 *         ) const
 *         e.g. int alloc_nodes = rdx->alloc_nodes();
 *
 *
 *         int
 *     bsize
 *         (
 *         ) const
 *         e.g. int bsize = rdx->bsize();
 *
 *
 *         int
 *     max_rdx_nodes
 *         (
 *         ) const
 *         e.g. int max_rdx_nodes = rdx->max_rdx_nodes();
 *
 *
 *         int
 *     num_keys
 *         (
 *         ) const
 *         e.g. int num_keys = rdx->num_keys();
 *
 *
 *         int
 *     max_key_bytes 
 *         (
 *         ) const
 *         e.g. int max_key_bytes = rdx->max_key_bytes();
 *
 *
 *         MKRdxPat<app_data> *
 *     chg_max_rdx_nodes 
 *         (
 *             const unsigned int new_max_rdx_nodes
 *         ) const
 *         e.g. MKRdxPat<app_data> *rdx_new = rdx_old->chg_max_rdx_nodes(new_max_rdx_nodes);
 *
 *
 *         int
 *     print
 *         (
 *             const unsigned char *key,  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
 *             ofstream& os
 *         )
 *         e.g. int return_code = rdx->print(NULL, os);
 *         e.g. int return_code = rdx->print((unsigned char *)key, os);
 *
 *
 *         int
 *     verify
 *         (
 *             VERIFY_MODE vm,
 *             ofstream& os
 *         )
 *         e.g. int return_code = rdx->verify(ERR_CODE, os);
 *         e.g. int return_code = rdx->verify(ERR_CODE_PRINT, os);
 *
 *======================================================================================================================
 *
 * Description:
 *     The MKRdxPat class allows the allocation of a fixed sized contiguous data store that holds data nodes
 *     of an arbitrary structure that may be accessed with any number of keys of any size with the PATRICIA
 *     (Practical Algorithm To Retrieve Information Coded In Alphanumeric)(1,2,3) fast search algorithm.
 *     MKRdxPat is particularly suited to applications that require complex data structures be contiguously
 *     stored and accessed with an algorithm of known fast character with any of several possible keys,
 *     either singly of several simultaneously.
 *
 *     For example, a data structure that required either of an IPv4, IPv6 or MAC address key(s) to access
 *     data nodes.  In this case NUM_KEYS would be 3 and MAX_KEY_BYTES would be 16(the longest - see below).
 *
 *     The MKRdxPat class supports a data structure of MAX_RDX_NODES data nodes and NUM_KEYS keys per
 *     data node with keys of MAX_KEY_BYTES bytes or less.  The class constructor:
 *
 *         MKRdxPat<app_data> *rdx = new MKRdxPat<app_data>(MAX_RDX_NODES, NUM_KEYS, MAX_KEY_BYTES);
 *
 *     along with a:
 *
 *         struct app_data
 *         {
 *             // user data
 *         };
 *
 *     class constructor typename specifies the data structure.  A data node may be accessed with a single
 *     key or any number of keys simultaneously.  Keys must be unique within their key index(0 - NUM_KEYS-1),
 *     but not over different key indexes.  Member functions that require keys are passed the array:
 *
 *         unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
 *
 *     One extra prefix byte, a key boolean - required to be 0(ignore) or 1(search), is set with each key to
 *     specify if the key is to be used in the data node search.  It is suggested that the array be first
 *     memset() to 0:
 *
 *         memset(key, 0, NUM_KEYS * (1+MAX_KEY_BYTES));
 *
 *     and then only keys to be used copied in with the key boolean(set to 1) prepended.
 *
 *     In most cases the necessary keys will be of different lengths.  The MAX_KEY_BYTES constructor argument
 *     would be set to the longest of these.  All other shorter keys would be left justified in the key[][]
 *     array, that is, start just after the key boolean.  Thus, for the example above, the IPv6 key would use
 *     the full 16 bytes, the IPv4 key would use the first four bytes after the key boolean(with the rest(12)
 *     set to 0) and the MAC key would use the first 6 bytes after the key boolean(with the rest(10) set to
 *     0).  This was done to make the specification of the key[][] array and the code associated with
 *     processing it simpler.  Actually, the IPv4 and MAC addresses could be right justified, just so long as
 *     all the keys are always right justified and the other bytes are 0; you just have to be consistent
 *     within any given key index.  From the way that PATRICIA works the keys are examined from left to
 *     right.  Thus, it makes sense to left justify.  Then, the actual key bits are examined instead of
 *     examining irrelevent 0 bits first.  The suffixed 0 bits are not a factor in the data node search since
 *     the key bits on the left will find a data node if all inserts were made with the same number of key
 *     bits on the left e.g. 32 for an IPv4 address.  Example:
 *
 *                 kb key bytes
 *         IPv4 :  01 c0 a8 00 01 00 00 00 00 00 00 00 00 00 00 00 00 - 192.168.0.1
 *         IPv6 :  01 fe 80 00 00 00 00 00 00 02 21 2f ff fe b5 6e 10 - fe80::221:2fff:feb5:6e10
 *         MAC  :  01 00 21 2f b5 6e 10 00 00 00 00 00 00 00 00 00 00 - 00:21:2f:b5:6e:10
 *
 *     Let's suppose that we wanted to find a data node with just the IPv6 address.  We first memset()
 *     key[][] to 0.  We then add the IPv6 address with it's key boolean set to 1 and pass the key[][] array
 *     to the member functions.  Thus:
 *
 *                 kb key bytes
 *         IPv4 :  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 - 0
 *         IPv6 :  01 fe 80 00 00 00 00 00 00 02 21 2f ff fe b5 6e 10 - fe80::221:2fff:feb5:6e10
 *         MAC  :  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 - 0
 *
 *     See the member function summary above or a detailed member function usage comment before each
 *     function's code.  A verify() member function is provided that does extensive data structure memory
 *     analysis and a print() member function is provided that prints the structural details of all the
 *     branch and data nodes.
 *
 *     For a trie of NUM_KEYS keys, each data node will have NUM_KEYS branch nodes associated with it.  The
 *     number of actual data nodes in the data structure is MAX_RDX_NODES+1.  This extra node is for the
 *     initial branch nodes and initial data node with the impossible key of all 0xff's(1,2,3).  The number
 *     of user storable nodes is MAX_RDX_NODES.   The user, through the constructor, declares PNODEs,
 *     each of which contains a radix PATRICIA trie of MAX_RDX_NODES nodes with NUM_KEYS keys of
 *     at most MAX_KEY_BYTES bytes length.
 *
 *     These routines are a modification of the algorithm cited.  Specifically, the upward pointers used to
 *     terminate search are not used, and are instead used to point to data nodes as trie leaves.  In
 *     multi-key search with PATRICIA the upward pointer scheme will not work since each key would have
 *     pointed to a different branch node with it's associated data structure.  In multi-key PATRICIA search
 *     all keys must lead to the same data node.  The viability of multi-key radix PATRICIA search depends
 *     on the fact that each data node insertion requires only one new branch node for each key, thus the
 *     initial allocation of NUM_KEYS branch nodes with each data node.  This implementation has several
 *     additional branch and data node members not used in the original PATRICIA search algorithm.  These
 *     are for various purposes; related to changes in the original algorithm for multi-key search, related
 *     to changes in the original algorithm for leaf data nodes instead of upward pointers to data structures
 *     stored in branch nodes, related to printing, related to sorting and debugging.
 *
 *======================================================================================================================
 *
 * Error Handling
 *
 *     1. Exceptions - The #include <exception> header is used to define exception objects throw()'n
 *                     upon calloc() failures.  Calloc() failures are thrown in the class constructor
 *                     MKRdxPat(), the chg_max_rdx_nodes() member function and the verify() member
 *                     function.  All other errors are reported via return values.
 *     2. Debugging  - Debugging output is provided for the class constructor - MKRdxPat() and member
 *                     functions insert(), search() and remove().  A debug() macro is provided.  It
 *                     will generte output if any one of three defines - DEBUG_I(insert()), DEBUG_S(search()),
 *                     DEBUG_R(remove()) - are set to 1.  If any of these are set to 1 then debug output for
 *                     MKRdxPat() will happen.
 *     3. Returns    - Member functions can return error indications.  These are documented in the comment
 *                     headings of the functions.  These are much faster than exceptions and should be
 *                     handled locally.
 *
 *======================================================================================================================
 *
 * Operational Notes:
 *
 *     1. the first member of BNODE and DNODE typedefs, id, should not be moved.  when doing operations on the
 *        trie, a search will reference the id from a BNODE type initially and then terminate at a DNODE type.
 *
 *     2. the maximum number of key bits is MAX_KEY_BITS.  the impossible key in the initially allocated data
 *        node must be at least one bit longer.  this requires the data node storage for the keys to have one
 *        extra byte.
 *
 *     3. all keys are the same length - MAX_KEY_BYTES bytes.  the performance hit, the small memory savings
 *        and the added code complexity of having individually sized keys did not seem worth it.  thus, the
 *        longest required key will determine MAX_KEY_BYTES.  shorter keys may be left justified in the
 *        MAX_KEY_BYTES bytes.
 *
 *     4. it is suggested that the 'unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]' array be first memset() to
 *        0 and then all the keys to be used with key boolean 1 added in one operation(keys with key boolean
 *        0 are never needed) e.g. memset(key, 0, NUM_KEYS * (1+MAX_KEY_BYTES));.
 *
 *     5. multidemensional arrays in C++ are implemented by subscripting calloc()'ed memory with parameterized
 *        values calculated from the C array versions.  One, two and three dimensional arrays are illustrated
 *        as follows(see verify() for actual usage):
 *
 *        n -> subscripts max_rdx_nodes_
 *        k -> subscripts num_keys_
 *        b -> subscripts max_key_bytes_:
 *
 *        a. one dimensional:
 *           unsigned long verify_dnode_addrs_[max_rdx_nodes_+1];
 *               -> verify_dnode_addrs_[n]
 *
 *        b. two dimensional:
 *           unsigned long verify_bnode_addrs_[num_keys_][max_rdx_nodes_+1];
 *               -> verify_bnode_addrs_[k*(max_rdx_nodes_+1)+n]
 *
 *        c. three dimensional:
 *           unsigned char verify_dnode_keys_[num_keys_][max_rdx_nodes_+1][max_key_bytes_];
 *               -> verify_dnode_keys_[(k*(max_rdx_nodes_+1)+n)*max_key_bytes_+b]
 *
 *======================================================================================================================
 *
 * Theoretic Notes:
 *
 *     1. depending on the key structure it is possible for the trie to be very unbalanced.  for a three byte
 *        key, for example, if 24 keys are inserted, each with only one different bit set, then the resulting
 *        trie would be a string of branch nodes extending to the left, each with a data node on its right
 *        branch.  the maximum trie depth would be 24 with the key of all zeros.
 *
 *     2. for the N node trie with b bit keys, even for very long keys, the average search requires log2(N)
 *        bit comparisons, and the average trie depth is log2(N).  see reference 1.  for a full key space,
 *        N = 2**b, log2(2**b) = b, and the average trie depth approaches b.
 *
 *     3. note that insertion requires two traversals of the trie - one to find if the key(s) already exists
 *        and the other to do the actual insertion.  the user may think that there must be a way to traverse
 *        down the trie only once to do an insertion.  the answer is YES! But.  in order to know where to
 *        stop you need to know all the key bits that intervene between the two b bit parameters of each
 *        branch node.  thus you would have to store these bits in each branch node or you could store any
 *        full key from any data node below a branch node in that branch node, which, by the property of a
 *        PATRICIA trie, will all have prefix bits identical for all data nodes below that branch node.  if,
 *        on insertion, the insertion key had bits differing between the two b bit parameters of two branch
 *        nodes then an insertion would be done at that point.  all this key storage and bit comparison in
 *        each traversed branch node is inefficient.  the first search traversal finds the key that does not
 *        match, but this key has all the key bits needed to find the first key bit of the new key that is
 *        different from the nearest key already in the trie.  this first differing bit then tells the second
 *        traversal where to stop and insert the new key.  thus, dual traversal is more efficient and simpler.
 *
 *======================================================================================================================
 *
 * References:
 *
 *     1. Algorithms in C++ Third Edition Parts 1-4, Robert Sedgewick with Christopher J. Van Wyk,
 *        Addison-Wesley, 1998, Chapter 15.3 Patricia Tries, pp. 637.
 *
 *     2. PATRICIA - Practical Algorithm To Retrieve Information Coded in Alphanumeric, Donald R. Morrison,
 *        Journal of the Association for Computing Machinery, Vol. 15, No. 4, Oct. 1968, pp. 514-534.
 *
 *     3. The Art of Computer Programming: Sorting and Searching, Donald E. Knuth,
 *        Addison-Wesley, 1973, pp 490-499.
 *
 *======================================================================================================================
 *
 * Author: Richard Hogaboom
 *         richard.hogaboom@gmail.com
 *
 *======================================================================================================================
 */

Constructor/Destructor:

/*
 *    struct app_data
 *    {
 *        int data;  // user specified application data structure
 *    };
 *
 *    MKRdxPat<app_data>
 *        (
 *            int MAX_RDX_NODES,
 *            int NUM_KEYS,
 *            int MAX_KEY_BYTES
 *        )
 *        e.g. MKRdxPat<app_data> *rdx = new MKRdxPat<app_data>(512, 3, 4);
 *
 *    ~MKRdxPat()
 *        e.g. delete rdx;
 */

Member Functions:

/*
 *======================================================================================================================
 *     insert()
 *
 * Purpose: 
 *     remove a data node from the free list in the PNODE_ data structure and insert this node into the trie with keys
 *     key[NUM_KEYS][1+MAX_KEY_BYTES].  the second subscript first byte for all keys(key[k][0]) is the key boolean.
 *     the key boolean should always be set to 1 since all keys are needed for insertion.  the actual key bytes
 *     (key[k][1 to MAX_KEY_BYTES]) follow the key boolean.  each key must be unique within it's key
 *     index(0 to NUM_KEYS-1).
 *
 * Usage:
 *     struct app_data
 *     {
 *         int data;
 *     }
 *
 *     unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES];
 *     app_data *app_datap;
 *     int return_code;
 *
 *     return_code = rdx->insert((unsigned char *)key, &app_datap);
 *
 * Returns:
 *     1. int return_code = 0, app_data *app_datap - if insertion is successful
 *     2. int return_code = 1, app_data *app_datap - if any key is found to already exist
 *     3. int return_code = 2, app_data *NULL - if no data nodes are on the free list
 *     4. int return_code = 3, app_data *NULL - if any key boolean is not 1
 *
 * Parameters:
 *     const unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES] - NUM_KEYS keys - one byte key boolean and MAX_KEY_BYTES key bytes
 *     app_data **app_datapp                              - pointer to pointer to the inserted data node app_data struct
 *                                                          or NULL if insertion fails
 *
 * Comments:
 */

    int
insert
    (
        const unsigned char *key,  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
        app_data **app_datapp
    )

/*
 *======================================================================================================================
 *     search()
 *
 * Purpose: 
 *     search trie for the data node with the keys key[NUM_KEYS][1+MAX_KEY_BYTES].  the second subscript first byte
 *     for all keys(key[k][0]) is the key boolean.  the key boolean should be set to 1(use the key) or
 *     0(do not use key).  the actual key bytes(key[k][1 to max_key_bytes_]) follow the key boolean.  thus, 1 to
 *     num_keys_ keys may be used in any given search.  only one key is necessary to find the data node.  each key
 *     must be unique within it's key index(0 - NUM_KEYS-1).
 *
 * Usage:
 *     unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES];
 *     app_data *app_datap;
 *
 *     app_datap = rdx->search((unsigned char *)key);
 *
 * Returns:
 *     1. app_data *app_datap - if search is successful
 *     2. app_data *NULL - if any key boolean is not 0 or 1
 *     3. app_data *NULL - if two key searches end at different data nodes
 *     4. app_data *NULL - if any key search does not end at a data node
 *     5. app_data *NULL - if no key boolean is 1 then no keys are used
 *    
 * Parameters:
 *     const unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES] - NUM_KEYS keys - one byte key boolean and MAX_KEY_BYTES key bytes
 *
 * Comments:
 *     e.g. set NUM_KEYS=3 and MAX_KEY_BYTES=4
 *
 *          example 1:
 *          in key[][]:
 *             key number  key
 *             0           01 aa bb cc dd    
 *             1           01 ee ee ee ee
 *             2           01 ff ff ff ff
 *
 *             use all three keys(0,1,2) in the search
 *
 *          example 2:
 *          in key[][]:
 *             0           00 aa bb cc dd
 *             1           01 ee ee ee ee
 *             2           00 ff ff ff ff
 *
 *             use only key 1 in the search
 *
 *          example 3:
 *          in key[][]:
 *             0           01 00 00 00 01
 *             1           01 00 00 00 01
 *             2           01 00 00 00 01
 *
 *             inserted successfuly - a. all key booleans 1  b. each key unique in it's own key index(0 - 2)
 */

    app_data *
search
    (
        const unsigned char *key  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
    )

/*
 *======================================================================================================================
 *     remove()
 *
 * Purpose: 
 *     remove trie data node with the keys key[NUM_KEYS][1+MAX_KEY_BYTES].  the second subscript first byte for all
 *     keys(key[k][0]) is the key boolean.  the key boolean should be set to 1(use the key) or 0(do not use key).
 *     the actual key bytes(key[k][1 to MAX_KEY_BYTES]) follow the key boolean.  thus, 1 to NUM_KEYS keys may be used
 *     for any given remove.  only one key is necessary to remove the data node.  each key must be unique within it's
 *     key index(0 - NUM_KEYS-1).
 *
 * Usage:
 *     unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES];
 *     app_data *app_datap;
 *
 *     app_datap = rdx->remove((unsigned char *)key);
 *
 * Returns:
 *     1. app_data *app_datap - if search is successful
 *     2. app_data *NULL - if any key boolean is not 0 or 1
 *     3. app_data *NULL - if two key searches end at different data nodes
 *     4. app_data *NULL - if any key search does not end at a data node
 *     5. app_data *NULL - if no key boolean is 1 then no keys are used
 *
 * Parameters:
 *     const unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES] - NUM_KEYS keys - one byte key boolean and MAX_KEY_BYTES key bytes
 *
 * Comments:
 *     e.g. set NUM_KEYS=3 and MAX_KEY_BYTES=4
 *
 *          example 1:
 *          in key[][]:
 *             key number  key
 *             0           01 aa bb cc dd    
 *             1           01 ee ee ee ee
 *             2           01 ff ff ff ff
 *
 *             use all three keys(0,1,2) in the search
 *
 *          example 2:
 *          in key[][]:
 *             0           00 aa bb cc dd
 *             1           01 ee ee ee ee
 *             2           00 ff ff ff ff
 *
 *             use only one key(1) in the search
 */

    app_data *
remove
    (
        const unsigned char *key  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
    )

/*
 *======================================================================================================================
 *     sort()
 *
 * Purpose: 
 *     sort pointers to app_data (app_data *app_data_ptrs[max_rdx_nodes_+1]) by key index k in key ascending order
 *
 * Usage:
 *     app_data **app_datapp;
 *     int k;
 *     int return_code;
 *
 *     return_code = rdx->sort(&app_datapp, k);
 *
 * Returns:
 *     1. int return_code = n, app_data **app_datapp - return the number of sorted nodes and pointer
 *        set to array of pointers to app_data
 *     2. int return_code = 0, app_data **NULL - if there are no data nodes(no keys to sort)
 *     3. int return_code = -1, app_data **NULL - if k, the key index, is out of range(0 - NUM_KEYS-1)
 *
 * Parameters:
 *     app_data ***app_data - pointer to pointer to pointer to app_data
 *     const int k          - key index(0 - NUM_KEYS-1)
 *
 * Comments:
 *     ((app_data *)app_datapp[0 to return_code-1])->(app_data struct fields) - to access app_data data
 *
 */

    int
sort
    (
        app_data ***app_datappp,
        const int k
    )

/*
 *======================================================================================================================
 *     keys()
 *
 * Purpose: 
 *     search for a data node with the key[][] argument which are a subset of the full set of keys and fill in
 *     key[][] with the data node keys which were not used in the search .  set the key boolean to 0 and copy the
 *     DNODE key to key[][].  for example, if three keys were used in the trie and the data node was found with only
 *     the first key then fill in key[][] with the second and third keys, leaving the key booleans for these keys
 *     set to 0.  thus, searching key[][] for key booleans of 0 will give all the keys not used in the original
 *     search.
 *
 * Usage:
 *     unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES];
 *     int return_code;
 *
 *     return_code = rdx->keys((unsigned char *)key);
 *
 * Returns:
 *     1. int return_code = 0 - data node found and missing keys filled in
 *     2. int return_code = 1 - can't find any data node with key[][]
 *     3. int return_code = 2 - no key[][]'s are missing to copy from DNODE
 *
 * Parameters:
 *     unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES] - NUM_KEYS keys - one byte key boolean and MAX_KEY_BYTES key bytes
 *
 * Comments:
 */

    int
keys
    (
        unsigned char *key  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
    )

/*
 *======================================================================================================================
 *     alloc_nodes()
 *
 * Purpose: 
 *     return the number of allocated data nodes in the trie.
 *
 * Usage:
 *     int alloc_nodes;
 *
 *     alloc_nodes = rdx->alloc_nodes();
 *
 * Returns:
 *     1. int alloc_nodes = rdx_.alloc_nodes - number of allocated nodes in the trie(0 - MAX_RDX_NODES)
 *
 * Parameters:
 *     None
 *
 * Comments:
 *     the number of actual nodes in the trie is MAX_RDX_NODES+1.  the extra node is the always allocated impossible
 *     key node with key(s) 0xff.  the maximum number of user available nodes is MAX_RDX_NODES.  this function
 *     returns only the number of user allocated nodes in the trie.
 */

    int
alloc_nodes
    (
    ) const

/*
 *======================================================================================================================
 *     bsize()
 *
 * Purpose: 
 *     return the size in bytes of the trie
 *
 * Usage:
 *     int bsize;
 *
 *     bsize = rdx->bsize();
 *
 * Returns:
 *     1. int bsize = rdx_.bsize - the size in bytes of the trie
 *
 * Parameters:
 *     None
 *
 * Comments:
 *     rdx trie size in bytes - includes all data and branch nodes including the root node
 */

    int
bsize
    (
    ) const

/*
 *======================================================================================================================
 *     max_rdx_nodes()
 *
 * Purpose: 
 *     return the maximum number of data nodes that may be allocated in the trie
 *
 * Usage:
 *     int max_rdx_nodes;
 *
 *     max_rdx_nodes = rdx->max_rdx_nodes();
 *
 * Returns:
 *     1. int max_rdx_nodes = max_rdx_nodes_ - maximum number of data nodes that may be allocated in the trie
 *
 * Parameters:
 *     None
 *
 * Comments:
 *     1. alloc_nodes() will always return 0 to max_rdx_nodes()
 */

    int
max_rdx_nodes
    (
    ) const

/*
 *======================================================================================================================
 *     num_keys()
 *
 * Purpose: 
 *     return the number of keys in the trie
 *
 * Usage:
 *     int num_keys;
 *
 *     num_keys = rdx->num_keys();
 *
 * Returns:
 *     1. int num_keys = num_keys_ - the number of keys in the trie
 *
 * Parameters:
 *     None
 *
 * Comments:
 */

    int
num_keys
    (
    ) const

/*
 *======================================================================================================================
 *     max_key_bytes()
 *
 * Purpose: 
 *     return the maximum number of bytes in any of the num_keys_ keys
 *
 * Usage:
 *     int max_key_bytes;
 *
 *     max_key_bytes = rdx->max_key_bytes();
 *
 * Returns:
 *     1. int max_key_bytes = max_key_bytes_ - the maximum number of bytes in any of the num_keys_ keys
 *
 * Parameters:
 *     None
 *
 * Comments:
 */

    int
max_key_bytes
    (
    ) const

/*
 *======================================================================================================================
 *     chg_max_rdx_nodes()
 *
 * Purpose: 
 *     create a new copy object of type MKRdxPat<app_data> with a new max_rdx_nodes_ and the same copied data.
 *     the new max_rdx_nodes_ value may be equal to the number of currently allocated nodes or larger.  thus, the
 *     new object can be shrunk to the size of the current number of allocated data nodes or expanded without limit.  
 *
 * Usage:
 *     unsigned int new_max_rdx_nodes;
 *
 *     MKRdxPat<app_data> *rdx_new = rdx_old->chg_max_rdx_nodes(new_max_rdx_nodes);
 *
 * Returns:
 *     1. MKRdxPat<app_data> *rdx_new - object of type MKRdxPat<app_data> containing the same data as the
 *                                      originating object with a new size limit
 *     2. MKRdxPat<app_data> *NULL - if new_max_rdx_nodes is less than the number of currently allocated nodes
 *     3. MKRdxPat<app_data> *NULL - if any transfer of a data node from the originating MKRdxPat<app_data> object
 *                                   to the new MKRdxPat<app_data> object fails
 *
 * Parameters:
 *     const unsigned int new_max_rdx_nodes - the new size, max_rdx_nodes_, of the returned object
 *
 * Comments:
 *     1. the originating object is not affected in any way
 */

    MKRdxPat<app_data> *
chg_max_rdx_nodes
    (
        const unsigned int new_max_rdx_nodes
    ) const

/*
 *======================================================================================================================
 *     print()
 *
 * Purpose: 
 *     Used in two modes:
 *         a. the first arg is NULL
 *
 *            print all the data in branch and data nodes for the entire trie.  for each data node allocated there
 *            will be NUM_KEYS new branch nodes allocated.  thus, for n data nodes allocated, there will be n*NUM_KEYS
 *            branch nodes printed and n data nodes printed.  the path along branch nodes to data nodes is not
 *            demonstrated.  useful only for small tries for debugging purposes.
 *
 *         b. the first arg is an array of 'unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]' of an existing allocated
 *            data node
 *
 *            print the data node and each branch node for each key(1 - NUM_KEYS) that leads to the found data node.
 *            the application data is not printed, only the trie structure data.
 *
 *     Setting 'unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]':
 *
 *     the second subscript first byte for all keys(key[k][0]) is the key boolean.  the key boolean should be set to
 *     1(use the key) or 0(do not use key).  the actual key bytes(key[k][1 to MAX_KEY_BYTES]) follow the key boolean.
 *     thus, 1 to NUM_KEYS keys may be used.  only one key is necessary to find the data node.  each key must be unique
 *     within it's key index(0 - NUM_KEYS-1).
 *
 * Usage:
 *     unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES];
 *     ofstream& os;
 *     int return_code;
 *
 *     return_code = rdx->print(NULL, os);
 *     return_code = rdx->print((unsigned char *)key, os);
 *
 * Returns:
 *     1. return_code = 0 - on success
 *     2. return_code = 1 - if data node for key[][] not found
 *
 * Parameters:
 *     const unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES] - NUM_KEYS keys - one byte key boolean and MAX_KEY_BYTES key bytes
 *     ofstream& os                                       - output stream
 *
 * Comments:
 *     e.g. set NUM_KEYS=3 and MAX_KEY_BYTES=4
 *
 *          example 1:
 *          in key[][]:
 *             key number  key
 *             0           01 aa bb cc dd    
 *             1           01 ee ee ee ee
 *             2           01 ff ff ff ff
 *
 *             use all three keys(0,1,2) in the search
 *
 *          example 2:
 *          in key[][]:
 *             0           00 aa bb cc dd
 *             1           01 ee ee ee ee
 *             2           00 ff ff ff ff
 *
 *             use only one key(1) in the search
 */

    int
print
    (
        const unsigned char *key,  // unsigned char key[NUM_KEYS][1+MAX_KEY_BYTES]
        ofstream& os
    )

/*
 *======================================================================================================================
 *     verify()
 *
 * Purpose: 
 *     verify the integrity of a PNODE_ structure.  multiple tests are used to verify the integrity of all branch and
 *     data node structures.  the input parameter enum VERIFY_MODE may be set to either ERR_CODE, which will
 *     return an integer error code(1-25) for each of the possible detected errors, or ERR_CODE_PRINT, which will
 *     return the same numerical error code and print to ofstream& os a text error message and current data
 *     structure data node addresses/keys and branch node addresses as well.
 *
 * Usage:
 *     ofstream& os;
 *     int return_code;
 *
 *     return_code = rdx->verify(ERR_CODE, os);
 *     return_code = rdx->verify(ERR_CODE_PRINT, os);
 *
 * Returns:
 *     1. return_code = 0 - if no error
 *     2. return_code = 1-25 - for the various errors
 *
 * Parameters:
 *     VERIFY_MODE vm  - enum with possible values (ERR_CODE, ERR_CODE_PRINT)
 *     ofstream& os    - output stream
 *
 * Comments:
 *     all error messages have the verify() file name and line number included
 */

    int
verify
    (
        VERIFY_MODE vm,
        ofstream& os
    )

```
