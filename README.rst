bloom-filter
============

A pure python bloom filter (low storage requirement, probabilistic
set datastructure) is provided.  It is known to work on CPython 3.x, Pypy,
and Jython.

Includes mmap, in-memory and disk-seek backends.

This project builds on `drs-bloom-filter` and `bloom_filter_mod`.
Credits and links can be found in AUTHORS.md.

Usage
-----

The user specifies the desired maximum number of elements and the
desired maximum false positive probability, and the module
calculates the rest.

.. code-block:: python

    from bloom_filter2 import BloomFilter

    # instantiate BloomFilter with custom settings,
    # max_elements is how many elements you expect the filter to hold.
    # error_rate defines accuracy; You can use defaults with
    # `BloomFilter()` without any arguments. Following example
    # is same as defaults:
    bloom = BloomFilter(max_elements=10000, error_rate=0.1)

    # Test whether the bloom-filter has seen a key:
    assert "test-key" not in bloom

    # Mark the key as seen
    bloom.add("test-key")

    # Now check again
    assert "test-key" in bloom

Bloom filter are pretty space efficient : only 200MB of memory usage for storing 100M elements with an error of 1%, compared to the 7GB required for set(range(10**8))

It still can be pretty useful to save/load to files with the mmap implementation, for example to avoid rebuilding the bloom filter. The `mmap <https://en.wikipedia.org/wiki/Mmap>`_ functionality also save some memory depending on system settings.

.. code-block:: python

    bloom = BloomFilter(max_elements=10**8, error_rate=0.01, filename=('/tmp/bloom.bin', -1))
