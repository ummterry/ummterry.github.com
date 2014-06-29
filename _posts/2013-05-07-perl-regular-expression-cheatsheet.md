---
layout: post
title: "Perl Regular Expression Cheatsheet"
description: ""
category: Other 
tags: [Perl Regex]
---
{% include JB/setup %}
A perl regular expression usually comes in something like this:
{% highlight perl %}
$string =~ m/PATTERN/i;
$string =~ s/PATTERN/NEW_PATTERN/i;
{% endhighlight %}
Here we divide the expression into 4 parts:

* =~	Match Operators, the operator between the variable and the expression
* m//	Quote-like Operators, appears after match operator
* /i	Options, the modifiers after the expression
* PATTERN	the Expression

# Match Operators

## =~

This operator appears between the string var you are comparing, and the regular expression you’re looking for (note that in selection or substitution a regular expression operates on the string var rather than comparing).

## !~

Just like =~, except negated. With matching, returns true if it DOESN’T match. I can’t imagine what it would do in translates, etc.

# Quote-like Operators

## qr/STRING/

This operator quotes (and possibly compiles) its STRING as a regular expression. STRING is interpolated the same way asPATTERN in m/PATTERN/. If “‘” is used as the delimiter, no interpolation is done. Returns a Perl value which may be used instead of the corresponding /STRING/msixpodual expression. The returned value is a normalized version of the original pattern. It magically differs from a string containing the same characters: ref(qr/x/) returns “Regexp”; however, dereferencing it is not well defined (you currently get the normalized version of the original pattern, but this may change).

## m/PATTERN/

## /PATTERN/

Searches a string for a pattern match, and in scalar context returns true if it succeeds, false if it fails. If no string is specified via the=~ or !~ operator, the $_ string is searched. (The string specified with =~ need not be an lvalue–it may be the result of an expression evaluation, but remember the =~ binds rather tightly.)

_The empty pattern //_

If the PATTERN evaluates to the empty string, the last successfully matched regular expression is used instead.

_Matching in list context_

If the /g option is not used, m// in list context returns a list consisting of the subexpressions matched by the parentheses in the pattern, that is, ($1, $2, $3…).

## m?PATTERN?

## ?PATTERN?

This is just like the m/PATTERN/ search, except that it matches only once between calls to the reset() operator.

## s/PATTERN/REPLACEMENT/

Searches a string for a pattern, and if found, replaces that pattern with the replacement text and returns the number of substitutions made. Otherwise it returns false (specifically, the empty string).

## \G assertion

You can intermix m//g matches with m/\G…/g, where \G is a zero-width assertion that matches the exact position where the previous m//g, if any, left off. Without the /g modifier, the \G assertion still anchors at pos() as it was at the start of the operation (see [“pos” in perlfunc](http://search.cpan.org/~rjbs/perl-5.16.3/pod/perlfunc.pod#pos)), but the match is of course only attempted once.

More details can be found [here](http://search.cpan.org/~rjbs/perl-5.16.3/pod/perlop.pod#Regexp_Quote-Like_Operators).

# Options

Options (specified by the following modifiers) are:

* m   Treat string as multiple lines.
* s   Treat string as single line. (Make . match a newline)
* i   Do case-insensitive pattern matching.
* x   Use extended regular expressions.
* p   Preserve a copy of the matched string in ${^PREMATCH}, ${^MATCH}, ${^POSTMATCH}
* o   Compile pattern only once.
* a   ASCII-restrict
* l   Use the locale
* u   Use Unicode rules
* d   Use Unicode or native charset, as in 5.12 and earlier
* g   Match globally, i.e., find all occurrences
* c   Do not reset search position on a failed match when /g is in effect
* e   Evaluate ‘replacement’ as an expression

# Expression Elements

## Basic Metacharacters

* . Match any single character except \n (unless /s)
* | OR; (ab|ac) matches ab or ac
* [abc] Match one out of a set of characters
* [^abc] Match one character not in set
* [a-z] Match one character from range, often [a-zA-Z]
* \ Escape next character, such as \/ or \( or \)

## Quantifiers

* * Match zero or more of previous character/subexpression
* + Match one or more of previous character/subexpression
* ? Match 0 or 1 of previous character/subexpression
* {n} Match exactly n of previous character/subexpression
* {m,n} Match m to n (inclusive) of previous character/subexp.
* {n,} Match n or more of previous character/subexpression
* *?, ?? Lazy version of same (works for any quantifier)
* *+, ?+ Possessive version (works for any quantifier)

## Specific Characters

* \w Word character (alphanumeric, underscore)
* \W Opposite of \w
* \s Whitespace character (space, tab, etc.)
* \S Opposite of \s
* \d Digit
* \D Opposite of \d
* [\b] Backspace (any use of \b in a character set)
* \n Newline
* \c Control character
* \f Form feed
* \r Carriage return
* \t Tab
* \v Vertical tab
* \x Hexadecimal number; \xf0 matches hex f0
* Octal number; 21 matches octal 21

## Anchors

* ^ Start of string (equivalent: $A unless /m is used)
* $ End of string (equivalent: $Z unless /m is used)
* \b Word boundary, similar to: (\w\W|\W\w)
* \B Anything but a word boundary

## Subexpressions

* ( ) Define a subexpression
* $a ath subexpression in or after substitution
* \a ath subexpression inside match operation
* (?:a) Non-capturing parentheses (match a)

## Case Conversion

* \l Make next character lowercase
* \u Make next character uppercase
* \L Make entire string (up to \E) lowercase
* \U Make entire string (up to \E) uppercase
* \E End \L or \U (so they only apply before \E)
* \u\L Capitalize first char, lowercase rest (sentence)

## Look-around

* ?= Look-ahead
* ?<= Look-behind