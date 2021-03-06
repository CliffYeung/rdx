```

 rdx-1.1.5.tar.gz - 2019/01/18
    a. upgraded g++/clang++ 
    b. fix bug in MKRdxPat_test.cpp.  two memset()s in test 15 zero out
       twice as much memory as they should.  MKRdxPat.hpp and the test
       were unaffected.  this bug was detected by the upgraded version
       of g++(clang++ did not detect the bug).
    c. fix bug in MKRdxPat_perf_bgp.cpp.  absent input file segfaults.
       add NULL test on FILE pointer.
    d. re-org rdx directory
    e. re-design doc

 rdx-1.1.4.tar.gz - 
    a. doc updates
    b. add #include <cmath> header to BGP performance measurement
       executable
    c. changes related to eliminating compiler warnings from
       updated compiler versions:
        1. add 'unsigned' to member function arg
               MKRdxPat<app_data> *
           chg_max_rdx_nodes(const unsigned int new_max_rdx_nodes) const
        2. eliminate using ' as a printf flag for comma separation of
           large integer numbers - not standard
        3. remove 'unsigned' from 'unsigned int' declaration of args
           to scanf() function since scanf() wants int* instead of
           unsigned int*

 rdx-1.1.3.tar.gz - 
    a. minor code changes to the test routines and documentation
       updates to MKRdxPat.hpp

 rdx-1.1.2.tar.gz - 
    a. a new executable - MKRdxPat_perf_bgp - has been added to
       measure performance with real BGP routing prefix data as keys
    b. minor mods to MKRdxPat_perf to improve outputs

 rdx-1.1.1.tar.gz - 
    a. add new member function, rdx->keys(key), to retrieve
       keys from a data node that were not used in the search
       for that data node

 rdx-1.1.0.tar.gz - 
    a. revise exception propagation
    b. improve debugging output
    c. add error handling doc section

 rdx-1.0.15.tar.gz - 
    a. minor code and documentation updates

 rdx-1.0.14.tar.gz - 
    a. bug fix - 'static's from C version eliminated from
       member functions
    b. three new accessor member functions to get the
       constructor arguments
    c. new member function to create and return a trie
       with the same allocated data nodes but a different
       maximum size.  the new trie may be smaller or larger
       than the original, but no smaller than the number of
       allocated nodes in the originating trie.
    d. new test for new member function to create resized
       clone of existing trie

 rdx-1.0.13.tar.gz - 
    a. minor code and documentation updates

 rdx-1.0.12.tar.gz - 
    a. minor code and documentation updates
    b. added new test case

 rdx-1.0.11.tar.gz - 
    a. change the license to the Boost 1.0 license

 rdx-1.0.10.tar.gz - 
    a. an OO C++ release
    b. a new performance measurement executable
    c. minor code and documentation updates
    d. no bugs were found

 rdx-1.0.9.tar.gz - 
    a. this is an update to the OO C++ version
    b. no bugs were found
    c. the APP_DATA.h file is gone. the constructor now is templetized to take a struct typename for the trie
       data nodes. this allows fully customized tries to be created in the same code block
    d. the test suite has been re-written, extended and each test is now fully independent of any other test
    e. the Documentation has been updated and substantially improved

 rdx-1.0.8.tar.gz - 
    a. an OO C++ release.  the test suite has been much improved for the OO C++ release.  the next release will
       further improve on the test suite.  an OO release allows multiply defined data structures in the same compilation unit:
           MKRdxPat *rdx0 = new MKRdxPat( 512 /* MAX_NUM_RDX_NODES */, 3 /* NUM_KEYS */, 4 /* NUM_KEY_BYTES */ );
           MKRdxPat *rdx1 = new MKRdxPat( 1024 /* MAX_NUM_RDX_NODES */, 2 /* NUM_KEYS */, 16 /* NUM_KEY_BYTES */ );

 rdx-1.0.7.tar.gz - 
    a. an OO C++ release.  the test suite is very basic; the next release will focus on an
       updated test suite.  an OO release allows multiply defined data structures in the same compilation unit:
           MKRdxPat *rdx0 = new MKRdxPat( 512 /* MAX_NUM_RDX_NODES */, 3 /* NUM_KEYS */, 4 /* NUM_KEY_BYTES */ );
           MKRdxPat *rdx1 = new MKRdxPat( 1024 /* MAX_NUM_RDX_NODES */, 2 /* NUM_KEYS */, 16 /* NUM_KEY_BYTES */ );

 rdx-1.0.6.tar.gz - 
    a. entirely a measuring performance release.  a new executable, rdx_pat_perf, is provided.  see
       the README.md file section under DESIGN GOALS on Performance.

 rdx-1.0.5.tar.gz - 
    a. new HISTORY.md file
    b. changed *test?.c file to not do some tests for one node/one key/one key byte case since
        these will always fail because these tests require three keys

 rdx-1.0.4.tar.gz - 
     a. fixed bug related to search/delete for a subset(1 to NUM_KEYS-1) of keys.  a byproduct of this
        fix is faster execution for both search/delete - fewer array accesses.
     b. the code is now compiled with -std=c11.  this required no changes, but some changes were
        made that proved useful e.g. for (int=0 ;;) and %p.
     c. the code for rdx_pat_print() and rdx_pat_verify() has been heavily updated to improve the
        print outs.
     d. all code, rdx_pat_search.c and test code, has been updated with many minor doc and formatting changes.
     e. the test code now comes in three versions.  All tests are the same, however, the trie is different
        in each case:
            1. 8 nodes, 3 keys, 4 bytes per key
            2. 1 nodes, 1 keys, 1 bytes per key
            3. 500 nodes, 5 keys, 12 bytes per key
        number 1 is the same as I have been using before.
        number 2 is the limiting case of 1 node with 1 key with 1 byte.
        number 3 is a test with a much larger trie.  this produces a 4M output file.
        running './rdx_pat.mk' will run all the tests.  the code and output is numbered by 1,2,3.
     f. the rdx_pat.mk script now has a clean mode.  do './rdx_pat.mk clean'.

 rdx-1.0.3.tar.gz - 
     a. modified insert to use the same type keys array with key boolean bytes required to be set to 1
        (use all keys).  this makes the the keys array arguments uniform over all routines.
     b. updated and simplified the test code the reflect the change to the insert routine.
     c. ran tests with much larger MAX_NUM_RDX_NODES, NUM_KEYS and NUM_KEY_BYTES.

 rdx-1.0.2.tar.gz - 
     a. search/delete/print now only require a subset of keys(from 1 to NUM_KEYS keys) - insert still requires
        a full set of keys of course.
     b. the distinction between C and C++ code is removed.  only one version is provided.  the rdx_pat.mk
        script can be run with clang, gcc or g++.
     c. the test suite has been improved substantially.  new tests for search/delete/print with a single key
        have been added.  the rdx_pat_test.results file of test output should be much more readable and the
        correspondence of results with example code should be clearer.

```
