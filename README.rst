PRSAnnots
=========

The PRSAnnots project exists to help you get annotated PDF files off
of your Sony PRS-T1 or PRS-T2 ereader.  These annotated versions
include both the freehand and highlighted annotations you made on
the reader.  To do this, PRSAnnots provides several programs_ as
well as a `Python package`_ to help you write your own.

PRSAnnots is alpha software.  There are many rough corners to cut
yourself on, so please be careful.  But rest assured it does very
little to the ereader itself, so it's unlikely for that to be broken.

.. contents:: **Contents**
   :local:

Programs
--------
PRSAnnots provides two command-line programs and a simple GUI script
to help manage your annotated PDF files.

prsam: The PRS Annotation Manager
'''''''''''''''''''''''''''''''''
The most featureful program, the annotation manager manages a
library of PDF files on your reader.  It notices when the
annotations of a given file have changed, and creates an annotated
PDF file on your computer with your up-to-date notes.  Here is some
basic usage::

  prsam init /media/READER

Initialize a library for a reader that is mounted at ``/media/READER``.

::

  prsam add path/to/file.pdf

Copy ``file.pdf`` to the reader, and add it to the library.

::

  prsam add path/to/twocolumn.pdf -d 2x2

Cut up each page of ``twocolumn.pdf`` into two columns and two rows,
so it's easier to read on the reader.  When synced back to the
computer, the PDF will be reassembled into full pages, with
annotations in the correct spots.

::

  prsam import download/file.pdf path/on/computer/

Add to the library ``file.pdf``, which is already in the
``download`` directory of the reader.  If ``file.pdf`` already has
annotations, it will be synced to ``path/on/computer/file.annot.pdf``.

::

  prsam import --all directory/

Add all *annotated* PDFs on the reader to the library.  They will
all be synced to ``directory/`` on your computer.

::

  prsam sync

Sync all PDFs with annotations changed since the last sync.

::

  prsam --help

Gives information on the various commands you can give ``prsam``.
This information is also available online_.

.. _online: https://github.com/rschroll/prsannots/wiki/All-Commands

prsam-tk: A graphical Annotation Manager
''''''''''''''''''''''''''''''''''''''''
``prsam-tk`` provides a `simple graphic interface`_ to the annotation
manager.  It provides a window where you can select PDF files to add
to the library, and it will help initialize a library if none
exists.  It lacks many of the features in ``prsam``.  But it uses
the same library, so you can switch between them as you like.

.. _simple graphic interface: https://github.com/rschroll/prsannots/wiki/Screenshot-Tour

getannotations: The simple, do-it-now approach
''''''''''''''''''''''''''''''''''''''''''''''
If you just want to get an annotated PDF file without messing with a
library manager, run this script with the mount point of your
ereader as the argument::

  getannotations /path/to/ereader

You'll be presented with a list of all PDF files with annotations on
the ereader; select the one you want and optionally specify the
filename to save it as.  The script will exit after creating the
annotated PDF file.

Python Package
--------------
The heavy-lifting of dealing with the annotated PDF files has been
pulled out of the programs above and put into the prsannots
package.  My hope is that this will allow the creation of better
GUIs, for normal people who aren't comfortable living on the command
line.

Here are the modules provided:

============= ==========================================================
manager       Provides the high-level library management of ``prsam``
              and ``prsam-tk``.
------------- ----------------------------------------------------------
generic       Provides abstractions for the reader, its books, and their
              annotations.  Planned to be useful for all Sony ereaders,
              but this has not been tested.
------------- ----------------------------------------------------------
prst1         Fills in the PRS-T1 and -T2 specific parts of generic.
------------- ----------------------------------------------------------
pdfannotation Create PDF annotations and add them to PDF files.
------------- ----------------------------------------------------------
pagetext      Get the location of each character on a PDF page, and get
              bounding boxes that contain certain characters.
------------- ----------------------------------------------------------
pdfdice       Cut the pages of a PDF file into subpages.
------------- ----------------------------------------------------------
pdfcontent    Adds content to PDF pages.  Includes a very simple SVG-to-
              PDF converter.
============= ==========================================================

Requirements
------------
You'll need some version of Python 2.  Thus far, PRSAnnots has been
tested on 2.6 and 2.7, but it should also work on 2.5.  (Can someone
confirm this?)  It will not work on 2.4 and earlier, and it is not
compatible with Python 3.

In addition to the standard library, it requires the pyPdf_ (>=
1.13) and PDFMiner_ libraries.  For Debian-based systems, the former
is provided by the ``python-pypdf`` package.  (Note that Debian
prior to Wheezy and Ubuntu prior to 11.10 have a too-old version of
pyPdf.)  Both are available in PyPI_. Depending on your
installation_ method, these may be installed for you.

.. _pyPDF: http://pybrary.net/pyPdf/
.. _PDFMiner: http://www.unixuser.org/~euske/python/pdfminer/
.. _PyPI: http://pypi.python.org/pypi

PRSAnnots has been developed and tested in Linux, but it should work
on any other operating system that can meet the above requirements.
I wouldn't be surprised if there were path or encoding problems on
Windows; please let me know and they should be fixable.

Installation
------------
The best way to get prsannots is by cloning the git repository::

  git clone git://github.com/rschroll/prsannots.git

Alternatively, you can download and unpack the tarball_ or zipball_.
All of the scripts may be run without installation, assuming you
have satisfied the requirements_.

.. _tarball: https://github.com/rschroll/prsannots/tarball/version-0.2.2
.. _zipball: https://github.com/rschroll/prsannots/zipball/version-0.2.2

You may install PRSAnnots with the ``setup.py`` script::

  python setup.py install

to install it globally on your system.  (You may need to be root.)
Or, to install it in your home directory::

  python setup.py install --home=~

Since Python package distribution is a mess_, I've tried to make
sure ``setup.py`` will work with distutils_, setuptools_,
distribute_, and pip_.  If you use any of the last three, the
dependencies should be installed automatically if they are needed.
If you're not sure what distribution systems you have installed,
just run ``setup.py``.  It will report at the end if there are
missing dependencies for you to install by hand.

.. _mess: http://guide.python-distribute.org/_images/state_of_packaging.jpg
.. _distutils: http://docs.python.org/distutils/index.html
.. _setuptools: http://pypi.python.org/pypi/setuptools
.. _distribute: http://packages.python.org/distribute/
.. _pip: http://www.pip-installer.org/en/latest/index.html

Alternative Windows Installer
'''''''''''''''''''''''''''''
We've put together a Windows installer that should take care of all
of dependencies for you.  You need to have Python installed first;
here are some `appropriate installers`_.  Then run the `PRSAnnots
installer`_.  It seems to run okay as a normal user, but you'll need
to run it as an Administrator in order to get the uninstaller to
work. (Note that you may get a warning from your SmartScreen filter
that the installer is "not commonly downloaded and not signed."  I
believe this basically mean that Microsoft has no idea whether the
installer is dangerous; it's not saying that something's wrong.)
The scripts are given appropriate file extensions and placed in
``<Python root>\Scripts`` (``C:\Python27\Scripts``, if you accept
the defaults).  If you can help make a better Windows installer,
please `let us know`_.

.. _appropriate installers: http://www.python.org/download/releases/2.7.3/
.. _PRSAnnots installer: https://github.com/rschroll/prsannots/releases/download/version-0.2.2/prsannots-0.2.2-x86_64.exe
.. _let us know: https://github.com/rschroll/prsannots/issues

Versions
--------
0.2.2 (2014/1/10):
  A few bug fixes and an update of the PDFMiner API.

0.2.1 (2012/11/17):
  Support for the PRS-T2, and a few minor bug fixes.

0.2 (2012/10/21):
  Adds cropping PDFs, previewing cropped and diced files, better
  handling of PDF annotations, and a number of bug fixes.

0.1.1 (2012/4/29):
  Fixes some Windows path issues.

0.1 (2012/4/28):
  Initial release.

Development
-----------
PRSAnnots is being developed on GitHub_.  Check out that site for
updated versions.  Please report bugs and feature requests to the
Github `bug tracker`_.

.. _GitHub: https://github.com/rschroll/prsannots
.. _bug tracker: https://github.com/rschroll/prsannots/issues

Limitations
'''''''''''
Annotation type:
  Both freehand and highlight annotations are supported.  Text notes
  attached to highlighted annotations are supported, but drawings
  are not.  The difficulty is in figuring out how to represent such
  notes in the PDF file.

Device support:
  Right now, only the Sony PRS-T1 and -T2 are supported, because
  that's what the author has.  From what I can tell, other Sony
  readers have similar schemes for their annotations, but store the
  information differently.  Adding support for these readers is
  hopefully as simple as producing an altered version of ``prst1.py``.

  I don't know how similarly other brands behave, but I'm happy to
  provide what assistance I can in trying to make them work.

Sync speed:
  Syncing PDFs may take a while (tens of seconds for short PDFs with
  few annotations).  This should be sped up, but I haven't figured
  out where the bottleneck is yet.  In the meantime, please be
  patient.

PDF viewers:
  The freehand annotations get written directly on the PDF file, and
  should be viewable in any PDF viewer.  Highlight annotations use
  the PDF annotation mechanism, which is supported by only some PDF
  viewers.  We ensure that all notations are visible in Acrobat
  Reader. Other viewers (notably Evince and Xpdf) do not display the
  highlight annotations.  For these, a "fake highlighting" mode can be
  enabled, adding a text annotation that is more widely supported.
  (Use ``prsam config --fake-highlight-on``.)

microSD cards:
  PRSAnnots only works with the files on the reader's main memory by
  default.  You may tell it to work with the files on the microSD
  card instead by specifying the mount point of the card instead of
  the mount point of the reader.  For getannotations, this works
  pretty well.  For prsam(-tk), it doesn't.  There is no way
  currently to deal with all the files at once.

About
-----
The wiki_ has some information on using PRSAnnots, and the `Google
Group`_ is a good place to ask questions and make comments.

.. _wiki: https://github.com/rschroll/prsannots/wiki
.. _Google Group: https://groups.google.com/d/forum/prsannots

PRSAnnots has been written (thus far) by Robert Schroll
(rschroll@gmail.com).  Feel free to get in touch with questions and
comments.
