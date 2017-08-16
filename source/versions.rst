
Versions
========

.. _Semantic Versioning: http://semver.org/

The *slimta* suite attempts to follow `Semantic Versioning`_. Basically, this
means that changes to the version components actually mean something definable.

The *python-slimta* project has reached the ``1.0.0`` milestone. This means
that all sub-versions of the MAJOR version component (``1.X.Y``) are backwards
compatible, while MINOR version changes may introduce non-breaking changes to
the API. Examples of non-breaking changes include new modules or classes, or
new optional arguments to existing functions and methods.

The other projects in the suite that are still version ``0.X.Y`` should be
considered in active development. For the most part, the MINOR version
component will still be used to enforce backwards-compatibility, but until the
project reaches ``1.0.0`` the API should be considered volatile.

Use of the word "stable" has so far been referring to how modifications are
made to the API. The ``1.0.0`` releases of *slimta* projects also try to carry
with it a commitment to application stability, particularly that production
traffic may be served with confidence.

.. note::

   Changing the order of or adding to optional parameters of a function or
   method is not considered a breaking change. Consider passing these arguments
   by keyword to avoid regressions. This choice helps maintain code cleanliness
   over time.

