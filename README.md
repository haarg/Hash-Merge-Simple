# NAME

Hash::Merge::Simple - Recursively merge two or more hashes, simply

# SYNOPSIS

```perl
use Hash::Merge::Simple qw/ merge /;

my $a = { a => 1 };
my $b = { a => 100, b => 2};

# Merge with righthand hash taking precedence
my $c = merge $a, $b;
# $c is { a => 100, b => 2 } ... Note: a => 100 has overridden => 1

# Also, merge will take care to recursively merge any subordinate hashes found
my $a = { a => 1, c => 3, d => { i => 2 }, r => {} };
my $b = { b => 2, a => 100, d => { l => 4 } };
my $c = merge $a, $b;
# $c is { a => 100, b => 2, c => 3, d => { i => 2, l => 4 }, r => {} }

# You can also merge more than two hashes at the same time
# The precedence increases from left to right (the rightmost has the most precedence)
my $everything = merge $this, $that, $mine, $yours, $kitchen_sink, ...;
```

# DESCRIPTION

Hash::Merge::Simple will recursively merge two or more hashes and return the result as a new hash reference. The merge function will descend and merge
hashes that exist under the same node in both the left and right hash, but doesn't attempt to combine arrays, objects, scalars, or anything else. The rightmost hash
also takes precedence, replacing whatever was in the left hash if a conflict occurs.

This code was pretty much taken straight from [Catalyst::Utils](https://metacpan.org/pod/Catalyst%3A%3AUtils), and modified to handle more than 2 hashes at the same time.

# USAGE

## Hash::Merge::Simple->merge( &lt;hash1>, &lt;hash2>, &lt;hash3>, ..., &lt;hashN> )

## Hash::Merge::Simple::merge( &lt;hash1>, &lt;hash2>, &lt;hash3>, ..., &lt;hashN> )

Merge &lt;hash1> through &lt;hashN>, with the nth-most (rightmost) hash taking precedence.

Returns a new hash reference representing the merge.

NOTE: The code does not currently check for cycles, so infinite loops are possible:

```perl
my $a = {};
$a->{b} = $a;
merge $a, $a;
```

NOTE: If you want to avoid giving/receiving side effects with the merged result, use `clone_merge` or `dclone_merge`
An example of this problem (thanks Uri):

```perl
my $left = { a => { b => 2 } } ;
my $right = { c => 4 } ;

my $result = merge( $left, $right ) ;

$left->{a}{b} = 3 ;
$left->{a}{d} = 5 ;

# $result->{a}{b} == 3 !
# $result->{a}{d} == 5 !
```

## Hash::Merge::Simple->clone\_merge( &lt;hash1>, &lt;hash2>, &lt;hash3>, ..., &lt;hashN> )

## Hash::Merge::Simple::clone\_merge( &lt;hash1>, &lt;hash2>, &lt;hash3>, ..., &lt;hashN> )

Perform a merge, clone the merge, and return the result

This is useful in cases where you need to ensure that the result can be tweaked without fear
of giving/receiving any side effects

This method will use [Clone](https://metacpan.org/pod/Clone) to do the cloning

## Hash::Merge::Simple->dclone\_merge( &lt;hash1>, &lt;hash2>, &lt;hash3>, ..., &lt;hashN> )

## Hash::Merge::Simple::dclone\_merge( &lt;hash1>, &lt;hash2>, &lt;hash3>, ..., &lt;hashN> )

Perform a merge, clone the merge, and return the result

This is useful in cases where you need to ensure that the result can be tweaked without fear
of giving/receiving any side effects

This method will use [Storable](https://metacpan.org/pod/Storable) (`dclone`) to do the cloning

# SEE ALSO

[Hash::Merge](https://metacpan.org/pod/Hash%3A%3AMerge)

[Catalyst::Utils](https://metacpan.org/pod/Catalyst%3A%3AUtils)

[Clone](https://metacpan.org/pod/Clone)

[Storable](https://metacpan.org/pod/Storable)

# ACKNOWLEDGEMENTS

This code was pretty much taken directly from [Catalyst::Utils](https://metacpan.org/pod/Catalyst%3A%3AUtils):

Sebastian Riedel `sri@cpan.org`

Yuval Kogman `nothingmuch@woobling.org`

# BUGS

Please report any bugs or feature requests on the bugtracker website
[https://rt.cpan.org/Public/Dist/Display.html?Name=Hash-Merge-Simple](https://rt.cpan.org/Public/Dist/Display.html?Name=Hash-Merge-Simple) or
by email to
[bug-Hash-Merge-Simple@rt.cpan.org](mailto:bug-Hash-Merge-Simple@rt.cpan.org).

When submitting a bug or request, please include a test-file or a
patch to an existing test-file that illustrates the bug or desired
feature.

# AUTHOR

Robert Krimen <robertkrimen@gmail.com>

# CONTRIBUTOR

Graham Knop <haarg@haarg.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2008 by Robert Krimen.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
