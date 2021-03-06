
==========
 Skyfield
==========

.. raw:: html

   <img class="logo" src="_static/logo.png">

.. rst-class:: motto

   *Elegant Astronomy for Python*

Skyfield computes positions for the stars and planets.
Its results should agree
with the positions generated by the United States Naval Observatory
and their *Astronomical Almanac*
to within at least 0.0005 arcseconds (half a “mas” or milliarcsecond).

* Written in pure Python and installs without any compilation.
* Supports Python 2.6–2.7 and Python 3.3–3.5.
* Only binary dependency is NumPy,
  the fundamental package for scientific computing with Python,
  whose vector operations make Skyfield efficient.

.. testsetup::

   __import__('skyfield.tests.fixes').tests.fixes.setup()

   import os
   os.chdir('../..')  # same directory as de430t.bsp, hopefully

.. testcode::

    from skyfield.api import load

    planets = load('de421.bsp')
    earth, mars = planets['earth'], planets['mars']

    ts = load.timescale()
    t = ts.now()
    astrometric = earth.at(t).observe(mars)
    ra, dec, distance = astrometric.radec()

    print(ra)
    print(dec)
    print(distance)

.. testoutput::

    10h 47m 56.24s
    +09deg 03' 23.1"
    2.33251 au

Skyfield can compute geocentric coordinates,
as shown in the example above,
or topocentric coordinates specific to your location
on the Earth’s surface:

.. testcode::

    boston = earth.topos('42.3583 N', '71.0636 W')
    astrometric = boston.at(t).observe(mars)
    alt, az, d = astrometric.apparent().altaz()

    print(alt)
    print(az)

.. testoutput::

    25deg 27' 54.0"
    101deg 33' 44.0"

Skyfield does not depend on the `AstroPy`_ project
or its compiled libraries.
But it understands how to use AstroPy time objects.
And it can convert results into native AstroPy units:

.. testcode::

    from astropy import units as u
    xyz = astrometric.position.to(u.au)
    altitude = alt.to(u.deg)

    print(xyz)
    print('{0:0.03f}'.format(altitude))

.. testoutput::

    [-2.19049548  0.71236701  0.36712443] AU
    25.465 deg

Documenation
============

Skyfield’s documentation lives here at the main Skyfield web site:

.. toctree::
   :maxdepth: 1

   toc
   api

But the source code and issue tracker live on other web sites:

* `Skyfield on the Python Package Index <https://pypi.python.org/pypi/skyfield>`_

* `GitHub project page <https://github.com/brandon-rhodes/python-skyfield/>`_

* `GitHub issue tracker <https://github.com/brandon-rhodes/python-skyfield/issues>`_

News
====

**2016 December 10**

  Released Skyfield 0.9.1
  which fixes an obscure module that,
  while not documented or supported at this point,
  would cause a ``SyntaxError`` when the Python package install tool
  would try to compile all of Skyfield’s ``.py`` files to ``.pyc`` files.

**2016 August 27**

  Released Skyfield 0.9
  which adds the ability of :ref:`turning-off-downloads`,
  offers an expanded :doc:`bibliography`,
  and provides several bugfixes.

**2016 March 30**

  Released Skyfield 0.8 with expanded documentation,
  that now includes an `api` presenting the docstrings
  from all stable and supported methods.

**2016 March 24**

  With the release of Skyfield 0.7,
  the final API upheavals of the pre-1.0 era are now complete.
  The introduction of the new timescale object
  has now eliminated all hidden state from the library,
  and has cleared the way for rapid development going forward.

  Unless users encounter significant problems,
  version 1.0 should follow as soon as the documentation —
  and in particular the API Reference —
  has received a bit more polish.
  The project is almost there!

.. testcleanup::

   __import__('skyfield.tests.fixes').tests.fixes.teardown()

.. _astropy: http://docs.astropy.org/en/stable/
