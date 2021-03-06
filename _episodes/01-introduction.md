---
title: "Introduction"
teaching: 10
exercises: 0
questions:
 - "Why Python?"
 - "What is uproot?"
 - "Why should I use it?"
objectives:
 - "Explain some advantages of Python and uproot."
keypoints:
 - "uproot is a fast and lightweight Python package for reading and writing ROOT files."
 - "uproot is designed to work efficiently with jagged arrays of data."
---

# Python and HEP

For quite a while now, ROOT has been the main software used by particle physicists for data analysis.
ROOT is written in C++, and at least until recently most analysis code has also been written in C++.
Outside of HEP, Python is a more commonly used language for data analysis, which means that there are many popular and well-supported tools that can be utilized for common tasks.
Python is also generally considered easier to learn than C++.

# uproot

uproot is a Python module that can read and write files in the ROOT format.
It's part of the Scikit-HEP project, a collection of Python tools for HEP data analysis.
uproot isn't the only way to read/write ROOT files in Python; there are other packages that allow you to do this.
However, there are some signficant advantages of uproot.
We'll go through a few here.

## Minimal dependencies

This is the entire list of requirements for uproot (other than Python itself, of course):

 - numpy
 - awkward
 - uproot-methods
 - cachetools

These are all Python modules, which can be installed with nothing more than `pip` or `conda`.
numpy is by far the most popular Python package for handling arrays of data.
It's included in the Anaconda distribution by default, and in general most people using Python for any kind of data analysis will probably already have it installed.
`awkward` and `uproot-methods` are also part of the Scikit-HEP project and can be thought of as effectively just subcomponents of uproot.
And finally, `cachetools` is a tiny package with some caching utilities.

Most importantly, uproot does not require any part of the ROOT software, which is quite large and can be non-trivial to install properly.

## Speed

uproot was designed for efficient reading of files.
It uses numpy for operations on data, which calls compiled vectorized functions to perform very fast calculations on arrays.

When you import a Python package that uses ROOT, there can be a delay of several seconds while all of the required libraries are loaded.
This is required every time you restart the Python interpreter or run a script.
By virtue of not using ROOT itself, uproot doesn't have this issue.

## Jagged arrays

One issue with manipulating HEP data in Python is that it is often not rectangular.
That is, the length of object collections is not constant.
For example, different events will have different numbers of jets, and different jets will have different numbers of associated tracks.
Packages like numpy are only designed to work with rectangular arrays, where each dimension has a fixed length throughout the entire array.
uproot uses a package called `awkward` to deal with non-rectangular (awkward) arrays so that they can be manipulated in ways almost identical to standard rectangular numpy arrays.
The most important class in `awkward` is called `JaggedArray`.

> ## Jagged vs. rectangular arrays
>
> The name `JaggedArray` comes from the idea that if you have a 2D array in which there's a different number of entries in each row and you line rows up by their first element, then the right edge of the array will be "jagged" as opposed to straight:
> 
> Rectangular array:
> 
> ~~~
> [[0, 1],
>  [2, 3],
>  [4, 5],
>  [6, 7],
>  [8, 9]
> ]
> ~~~
> {: .source}
> 
> Jagged array:
> 
> ~~~
> [[0, 1],
>  [2, 3, 4],
>  [5],
>  [6, 7],
>  [8, 9],
> ]
> ~~~
> {: .source}
{: .callout}

Other Python packages for reading ROOT files usually do not support jagged arrays, which means that you cannot do operations on multiple events at once if you have jagged data.
But uproot can, and this is generally much faster than operating on one event at a time. We'll come back to this topic in the "Columnar Analysis" section.

{% include links.md %}
