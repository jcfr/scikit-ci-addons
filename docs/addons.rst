=======
Add-ons
=======

Each category is named after a CI worker (e.g appveyor) and references add-ons
designed to be used on the associated continuous integration service.

An add-on is a file that could either directly be executed or used as a
parameter for an other tool.


Anyci
-----

This a special category containing scripts that could be executed on a broad
range of CI services.


``noop.py``
^^^^^^^^^^^

Display name of script and associated argument (basically the value of
``sys.argv``).


``run.sh``
^^^^^^^^^^

Wrapper script executing command and arguments passed as parameters.


Appveyor
--------

These scripts are designed to work on worker from http://appveyor.com/


``install_cmake.py``
^^^^^^^^^^^^^^^^^^^^

Download and install in the PATH the specified version of CMake binaries.

Usage::

  python appveyor/install_cmake.py X.Y.Z

Example::

  python appveyor/install_cmake.py 3.6.2

Notes:

- CMake archive is downloaded and extracted into ``C:\\cmake-X.Y.Z``. That
  same directory can then be added to the cache. See `Build Cache <https://www.appveyor.com/docs/build-cache/>`_
  documentation for more details.

- ``C:\\cmake-X.Y.Z`` is prepended to the ``PATH``.
  TODO: Is the env global on AppVeyor ? Or does this work only with scikit-ci ?



``run-with-visual-studio.cmd``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is a wrapper script setting the Visual Studio environment
matching the selected version of Python. This is particularly
important when building Python C Extensions.


Usage::

  run-with-visual-studio.cmd \\path\\to\\command [arg1 [...]]

Example::

  SET PYTHON_DIR="C:\\Python35"
  SET PYTHON_VERSION="3.5.x"
  SET PYTHON_ARCH="64"
  SET PATH=%PYTHON_DIR%;%PYTHON_DIR%\\Scripts;%PATH%
  run-with-visual-studio.cmd python setup.by bdist_wheel


Notes:

- Python version selection is done by setting the ``PYTHON_VERSION`` and
  ``PYTHON_ARCH`` environment variables.

- Possible values for  ``PYTHON_VERSION`` are:

  - ``"2.7.x"``

  - ``"3.4.x"``

  - ``"3.5.x"``

- Possible values for ``PYTHON_ARCH`` are:

  - ``"32"``

  - ``"64"``

Author:

-  Olivier Grisel

License:

- `CC0 1.0 Universal <http://creativecommons.org/publicdomain/zero/1.0/>`_



``patch_vs2008.py``
^^^^^^^^^^^^^^^^^^^

This script patches the installation of `Visual C++ 2008 Express <https://www.appveyor.com/docs/installed-software/#visual-studio-2008>`_
so that it can be used to build 64-bit projects.

Usage::

  python appveyor/patch_vs2008.py

Notes:

- Download `vs2008_patch.zip <https://github.com/menpo/condaci/raw/master/vs2008_patch.zip>`_
  and execute ``setup_x64.bat``.

Credits:

- Xia Wei, sunmast#gmail.com

Links:

- http://www.cppblog.com/xcpp/archive/2009/09/09/vc2008express_64bit_win7sdk.html


``tweak_environment.py``
^^^^^^^^^^^^^^^^^^^^^^^^

Usage::

  python tweak_environment.py

Notes:

- Update ``notepad++`` settings:

  - ``TabSetting.replaceBySpace`` set to ``yes``


Circle
------

These scripts are designed to work on worker from http://circleci.com/

``install_cmake.py``
^^^^^^^^^^^^^^^^^^^^

Download and install in the PATH the specified version of CMake binaries.

Usage::

  python appveyor/install_cmake.py X.Y.Z

Example::

  python appveyor/install_cmake.py 3.6.2

Notes:

- The script will skip the download if current version matches the selected
  one.


Travis
------

These scripts are designed to work on worker from http://travis-ci.org/

``install_cmake.py``
^^^^^^^^^^^^^^^^^^^^

Download and install in the PATH the specified version of CMake binaries.

Usage::

  python appveyor/install_cmake.py X.Y.Z

Example::

  python appveyor/install_cmake.py 3.6.2


Notes:

- The script automatically detects the operating system (``linux`` or ``osx``)
  and install CMake in a valid location.

- The archives are downloaded in ``/home/travis/downloads`` to allow
  caching. See `Caching Dependencies and Directories <https://docs.travis-ci.com/user/caching/>`_
  The script the download if the correct CMake archive is found in ``/home/travis/downloads``.

- Linux:

  - To support worker with and without ``sudo`` enabled, CMake is installed
    in ``HOME`` (i.e /home/travis). Since ``~/bin`` is already in the ``PATH``,
    CMake executables will be available in the PATH after running this script.

- MacOSX:

  - Consider using this script only if the available version does **NOT**
    work for you. See the `Compilers-and-Build-toolchain <https://docs.travis-ci.com/user/osx-ci-environment/#Compilers-and-Build-toolchain>`_
    in Travis documentation.

  - What does this script do ? First, it removes the older version of CMake
    executable installed in ``/usr/local/bin``. Then, it installs the selected
    version of CMake using ``sudo cmake-gui --install``.



``install_pyenv.py``
^^^^^^^^^^^^^^^^^^^^

Usage::

  export PYTHONVERSION=X.Y.Z
  python install_pyenv.py

Notes:

- Update the version of ``pyenv`` using ``brew``.

- Install the version of python selected setting ``PYTHONVERSION``
  environment variable.


``run-with-pyenv.sh``
^^^^^^^^^^^^^^^^^^^^^

This is a wrapper script setting the environment corresponding to the
version selected setting ``PYTHONVERSION`` environment variable.

Usage::

  export PYTHONVERSION=X.Y.Z
  run-with-pyenv.sh python --version