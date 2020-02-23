---
title: perl notes
date: 2020-02-15 00:54:07
categories:
- programing languages
tags:
- perl
---

I got a job at utsc to improve the website of a stats course. The pay is good, so I accepted the offer. It is just that they are using webwork, a web framework for math courses. It is written in perl and latex, so I have to pick perl up in order to finish the job. Perl is an interesting language anyway. Its syntax is way too wired just like haskell (even more strange)

<!--more-->

# Regular Expression

## Modifiers

+ **/g**: match all occurences of the pattern
+ **/i**: ignore cases
+ ($s =~ s/././gi): returns the number of substitutions made
+ **tr///**: used to substitude one by one

# Syntax

## Create multi-line string

```perl
my $s = <<END;
This is the first line.
This is the second line.
The last line.
END
```

# Builtin Functions

+ **chomp**: remove the last char of the string and return the number of chars deleted

