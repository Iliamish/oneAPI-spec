.. SPDX-FileCopyrightText: 2019-2020 Intel Corporation
..
.. SPDX-License-Identifier: CC-BY-4.0

===================
concurrent_multiset
===================
**[containers.concurrent_multiset]**

``tbb::concurrent_multiset`` is a class template that represents an sorted sequence of elements.
It supports concurrent insertion, lookup, and traversal, but does not support concurrent erasure.
In this container, multiple equivalent elements can be stored.


Class Template Synopsis
-----------------------

.. code:: cpp

    // Defined in header <tbb/concurrent_set.h>

    namespace tbb {

        template <typename T,
                  typename Compare = std::less<T>,
                  typename Allocator = tbb_allocator<T>>
        class concurrent_multiset {
        public:
            using key_type = T;
            using value_type = T;

            using size_type = <implementation-defined unsigned integer type>;
            using difference_type = <implementation-defined signed integer type>;

            using key_compare = Compare;
            using value_compare = Compare;

            using allocator_type = Allocator;

            using reference = value_type&;
            using const_reference = const value_type&;
            using pointer = std::allocator_traits<Allocator>::pointer;
            using const_pointer = std::allocator_traits<Allocator>::const_pointer;

            using iterator = <implementation-defined ForwardIterator>;
            using const_iterator = <implementation-defined constant ForwardIterator>;

            using node_type = <implementation-defined node handle>;

            using range_type = <implementation-defined range>;
            using const_range_type = <implementation-defined constant node handle>;

            // Construction, destruction, copying
            concurrent_multiset();
            explicit concurrent_multiset( const key_compare& comp,
                                          const allocator_type& alloc = allocator_type() );

            explicit concurrent_multiset( const allocator_type& alloc );

            template <typename InputIterator>
            concurrent_multiset( InputIterator first, InputIterator last,
                                 const key_compare& comp = key_compare(),
                                 const allocator_type& alloc = allocator_type() );

            template <typename InputIterator>
            concurrent_multiset( InputIterator first, InputIterator last,
                                 const allocator_type& alloc );

            concurrent_multiset( std::initializer_list<value_type> init,
                                 const key_compare& comp = key_compare(),
                                 const allocator_type& alloc = allocator_type() );

            concurrent_multiset( std::initializer_list<value_type> init, const allocator_type& alloc );

            concurrent_multiset( const concurrent_multiset& other );
            concurrent_multiset( const concurrent_multiset& other,
                                 const allocator_type& alloc );

            concurrent_multiset( concurrent_multiset&& other );
            concurrent_multiset( concurrent_multiset&& other,
                                 const allocator_type& alloc );

            ~concurrent_multiset();

            concurrent_multiset& operator=( const concurrent_multiset& other );
            concurrent_multiset& operator=( concurrent_multiset&& other );
            concurrent_multiset& operator=( std::initializer_list<value_type> init );

            allocator_type get_allocator() const;

            // Iterators
            iterator begin();
            const_iterator begin() const;
            const_iterator cbegin() const;

            iterator end();
            const_iterator end() const;
            const_iterator cend() const;

            // Size and capacity
            bool empty() const;
            size_type size() const;
            size_type max_size() const;

            // Concurrently safe modifiers
            std::pair<iterator, bool> insert( const value_type& value );

            iterator insert( const_iterator hint, const value_type& value );

            std::pair<iterator, bool> insert( value_type&& value );

            iterator insert( const_iterator hint, value_type&& value );

            template <typename InputIterator>
            void insert( InputIterator first, InputIterator last );

            void insert( std::initializer_list<value_type> init );

            std::pair<iterator, bool> insert( node_type&& nh );
            iterator insert( const_iterator hint, node_type&& nh );

            template <typename... Args>
            std::pair<iterator, bool> emplace( Args&&... args );

            template <typename... Args>
            iterator emplace_hint( const_iterator hint, Args&&... args );

            template <typename SrcCompare>
            void merge( concurrent_set<T, SrcCompare, Allocator>& source );

            template <typename SrcCompare>
            void merge( concurrent_set<T, SrcCompare, Allocator>&& source );

            template <typename SrcCompare>
            void merge( concurrent_multiset<T, SrcCompare, Allocator>& source );

            template <typename SrcCompare>
            void merge( concurrent_multiset<T, SrcCompare, Allocator>&& source );

            // Concurrently unsafe modifiers
            void clear();

            iterator unsafe_erase( const_iterator pos );
            iterator unsafe_erase( iterator pos );

            iterator unsafe_erase( const_iterator first, const_iterator last );

            size_type unsafe_erase( const key_type& key );

            template <typename K>
            size_type unsafe_erase( const K& key );

            node_type unsafe_extract( const_iterator pos );
            node_type unsafe_extract( iterator pos );

            node_type unsafe_extract( const key_type& key );

            template <typename K>
            node_type unsafe_extract( const K& key );

            void swap( concurrent_multiset& other );

            // Lookup
            size_type count( const key_type& key );

            template <typename K>
            size_type count( const K& key );

            iterator find( const key_type& key );
            const_iterator find( const key_type& key ) const;

            template <typename K>
            iterator find( const K& key );

            template <typename K>
            const_iterator find( const K& key ) const;

            bool contains( const key_type& key ) const;

            template <typename K>
            bool contains( const K& key ) const;

            std::pair<iterator, iterator> equal_range( const key_type& key );
            std::pair<const_iterator, const_iterator> equal_range( const key_type& key ) const;

            template <typename K>
            std::pair<iterator, iterator>  equal_range( const K& key );
            std::pair<const_iterator, const_iterator> equal_range( const K& key ) const;

            iterator lower_bound( const key_type& key );
            const_iterator lower_bound( const key_type& key ) const;

            template <typename K>
            iterator lower_bound( const K& key );

            template <typename K>
            const_iterator lower_bound( const K& key ) const;

            iterator upper_bound( const key_type& key );
            const_iterator upper_bound( const key_type& key ) const;

            template <typename K>
            iterator upper_bound( const K& key );

            template <typename K>
            const_iterator upper_bound( const K& key ) const;

            // Observers
            key_compare key_comp() const;

            value_compare value_comp() const;

            // Parallel iteration
            range_type range();
            const_range_type range() const;
        }; // class concurrent_multiset

    } // namespace tbb


Requirements:

* The expression ``std::allocator_traits<Allocator>::destroy(m, val)``, where ``m`` is an object
  of the type ``Allocator`` and ``val`` is an object of the type ``value_type``, should be well-formed.
  Member functions can impose stricter requirements depending on the type of the operation.
* The type ``Compare`` must meet the ``Compare`` requirements from the [alg.sorting] ISO C++ Standard section.
* The type ``Allocator`` must meet the ``Allocator`` requirements from the [allocator.requirements] ISO C++ Standard section.


Member functions
----------------

.. toctree::
    :maxdepth: 1

    concurrent_multiset_cls/construction_destruction_copying.rst
    concurrent_multiset_cls/iterators.rst
    concurrent_multiset_cls/size_and_capacity.rst
    concurrent_multiset_cls/safe_modifiers.rst
    concurrent_multiset_cls/unsafe_modifiers.rst
    concurrent_multiset_cls/lookup.rst
    concurrent_multiset_cls/observers.rst
    concurrent_multiset_cls/parallel_iteration.rst

Non-member functions
--------------------

These functions provides binary and lexicographical comparison and swap operations
on ``tbb::concurrent_multiset`` objects.

The exact namespace where these functions are defined is unspecified, as long as they may be used in
respective comparison operations. For example, an implementation may define the classes and functions
in the same internal namespace and define ``tbb::concurrent_multiset`` as a type alias for which
the non-member functions are reachable only via argument dependent lookup.

.. code:: cpp

    template <typename T, typename Compare, typename Allocator>
    void swap( concurrent_multiset<T, Compare, Allocator>& lhs,
               concurrent_multiset<T, Compare, Allocator>& rhs );

    template <typename T, typename Compare, typename Allocator>
    bool operator==( const concurrent_multiset<T, Compare, Allocator>& lhs,
                     const concurrent_multiset<T, Compare, Allocator>& rhs );

    template <typename T, typename Compare, typename Allocator>
    bool operator!=( const concurrent_multiset<T, Compare, Allocator>& lhs,
                     const concurrent_multiset<T, Compare, Allocator>& rhs );

    template <typename T, typename Compare, typename Allocator>
    bool operator<( const concurrent_multiset<T, Compare, Allocator>& lhs,
                    const concurrent_multiset<T, Compare, Allocator>& rhs );

    template <typename T, typename Compare, typename Allocator>
    bool operator>( const concurrent_multiset<T, Compare, Allocator>& lhs,
                    const concurrent_multiset<T, Compare, Allocator>& rhs );

    template <typename T, typename Compare, typename Allocator>
    bool operator<=( const concurrent_multiset<T, Compare, Allocator>& lhs,
                     const concurrent_multiset<T, Compare, Allocator>& rhs );

    template <typename Key, typename T, typename Compare, typename Allocator>
    bool operator>=( const concurrent_multiset<T, Compare, Allocator>& lhs,
                     const concurrent_multiset<T, Compare, Allocator>& rhs );

.. toctree::
    :maxdepth: 1

    concurrent_multiset_cls/non_member_swap.rst
    concurrent_multiset_cls/non_member_binary_comparisons.rst
    concurrent_multiset_cls/non_member_lexicographical_comparisons.rst

Other
-----

.. toctree::
    :maxdepth: 1

    concurrent_multiset_cls/deduction_guides.rst