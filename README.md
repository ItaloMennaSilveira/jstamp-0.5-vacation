# jstamp-0.5-vacation

Version
-------
The version of this benchmark was developed by the research group called Programming Languages Research Group (http://plrg.eecs.uci.edu/software_page/) and is being used as the basis for the implementation of two other new releases.


Introduction
------------

This benchmark implements a travel reservation system powered by a
non-distributed database. The workload consists of several client threads
interacting with the database via the system's transaction manager.

The database is consists of four tables: cars, rooms, flights, and customers.
The first three have relations with fields representing a unique ID number,
reserved quantity, total available quantity, and price. The table of customers
tracks the reservations made by each customer and the total price of the
reservations they made. The tables are implemented as Red-Black trees.

This benchmark

When using this benchmark, please cite [1].


Compiling and Running
---------------------

To build the application, simply run:

    make -f <makefile>

in the source directory. For example, for the sequential flavor, run:

    make -f Makefile.seq

By default, this produces an executable named "vacation", which can then be
run in the following manner:

    ./vacation -n <number_of_queries_per_task> \
               -q <%_of_relations_queried> \
               -r <number_possible_relations> \
               -u <%_of_user_tasks> \
               -t <number_of_tasks>

The following values are recommended for simulated runs:

    low contention:  -n2 -q90 -u98 -r16384 -t4096
    high contention: -n4 -q60 -u90 -r16384 -t4096

For non-simulator runs, larger values for -r and -t can be used:

    low contention:  -n2 -q90 -u98 -r1048576 -t4194304
    high contention: -n4 -q60 -u90 -r1048576 -t4194304


Workload Characteristics
------------------------

The initial size of the database is determined by -r, which is the number of
entries with which the tree will be initialized. The number of total tasks that
the travel reservation system performs is controlled by -t. There are four
different possible tasks:

    1) Make Reservation -- The client checks the price of -n items, and
       reserves a few of them.

    2) Delete Customer -- The total cost of a customer's reservations is
       computed and then the customer is removed from the system.

    3) Add to Item Tables -- Add -n new items for reservation, where
       an item is one of {car, flight, room} and has a unique ID number.

    4) Remove from Item Tables -- Remove -n new items for reservation, where
       an item is one of {car, flight, room} and has a unique ID number.

The distribution of tasks can be adjusted with the -u option. For example,
an argument of U gives the following distribution:

    U % ---------- Make Reservation
    (100-U)/2 % -- Delete Customer
    (100-U)/4 % -- Add to Item Tables
    (100-U)/4 % -- Remove from Item Tables

The -q option controls the range of values from which the clients generate
queries; thus, smaller values for -q generate higher contention workloads.


References
----------

[1] C. Cao Minh, J. Chung, C. Kozyrakis, and K. Olukotun. STAMP: Stanford 
    Transactional Applications for Multi-processing. In IISWC '08: Proceedings
    of The IEEE International Symposium on Workload Characterization,
    September 2008. 
