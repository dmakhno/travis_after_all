travis_after_all 
================

[![Build Status](https://travis-ci.org/dmakhno/travis_after_all.png?branch=master)](https://travis-ci.org/dmakhno/travis_after_all)

This is travis helper to run particular work only once in matrix.

This is workaround for: https://github.com/travis-ci/travis-ci/issues/929

The main goal of this script to have single publishing when build have several jobs.

Limitations/Todo
----------------

- If other jobs will start late, can be global build timeout. (Think of passing leader role to others)
- More flexible leader defenition (In Matrix not all jobs can publish)
- Have several leaders, according to flavours (E.g. if in Matrix slices responsible for platform)