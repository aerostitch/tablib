.. _formats:

=======
Formats
=======

Tablib supports a wide variety of different tabular formats, both for input and
output. Moreover, you can :ref:`register your own formats <newformats>`.

csv
===

When you import CSV data, you can specify if the first line of your data source
is headers with the ``headers`` boolean parameter (defaults to ``True``)::

    import tablib

    tablib.import_set(your_data_stream, format='csv', headers=False)

When exporting with the ``csv`` format, the top row will contain headers, if
they have been set. Otherwise, the top row will contain the first row of the
dataset.

When importing a CSV data source or exporting a dataset as CSV, you can pass any
parameter supported by the :py:func:`csv.reader` and :py:func:`csv.writer`
functions. For example::

    tablib.import_set(your_data_stream, format='csv', dialect='unix')

    dataset.export('csv', delimiter=' ', quotechar='|')

.. admonition:: Line endings

     Exporting uses \\r\\n line endings by default so, make sure to include
     ``newline=''`` otherwise you will get a blank line between each row
     when you open the file in Excel::

         with open('output.csv', 'w', newline='') as f:
             f.write(dataset.export('csv'))

     If you do not do this, and you export the file on Windows, your
     CSV file will open in Excel with a blank line between each row.

dbf
===

Import/export using the dBASE_ format.

.. admonition:: Binary Warning

    The ``dbf`` format contains binary data, so make sure to write in binary
    mode::

        with open('output.dbf', 'wb') as f:
            f.write(dataset.export('dbf')

.. _dBASE: https://en.wikipedia.org/wiki/DBase

df (DataFrame)
==============

Import/export using the pandas_ DataFrame format. This format is optional,
install Tablib with ``pip install tablib[pandas]`` to make the format available.

.. _pandas: https://pandas.pydata.org/

html
====

The ``html`` format is currently export-only. The exports produce an HTML page
with the data in a ``<table>``. If headers have been set, they will be used as
table headers.

This format is optional, install Tablib with ``pip install tablib[html]`` to
make the format available.

jira
====

The ``jira`` format is currently export-only. Exports format the dataset
according to the Jira table syntax::

    ||heading 1||heading 2||heading 3||
    |col A1|col A2|col A3|
    |col B1|col B2|col B3|

json
====

Import/export using the JSON_ format. If headers have been set, a JSON list of
objects will be returned. If no headers have been set, a JSON list of lists
(rows) will be returned instead.

Import assumes (for now) that headers exist.

.. _JSON: http://json.org/

latex
=====

Import/export using the LaTeX_ format. This format is export-only.
If a title has been set, it will be exported as the table caption.
 
.. _LaTeX: https://www.latex-project.org/

ods
===

Export data in OpenDocument Spreadsheet format. The ``ods`` format is currently
export-only.

This format is optional, install Tablib with ``pip install tablib[ods]`` to
make the format available.

.. admonition:: Binary Warning

    :class:`Dataset.ods` contains binary data, so make sure to write in binary mode::

        with open('output.ods', 'wb') as f:
            f.write(data.ods)

rst
===

Export data as a reStructuredText_ table representation of a dataset. The
``rst`` format is export-only.

Exporting returns a simple table if the text in the first column is never
wrapped, otherwise returns a grid table::

    >>> from tablib import Dataset
    >>> bits = ((0, 0), (1, 0), (0, 1), (1, 1))
    >>> data = Dataset()
    >>> data.headers = ['A', 'B', 'A and B']
    >>> for a, b in bits:
    ...     data.append([bool(a), bool(b), bool(a * b)])
    >>> table = data.export('rst')
    >>> table.split('\\n') == [
    ...     '=====  =====  =====',
    ...     '  A      B    A and',
    ...     '                B  ',
    ...     '=====  =====  =====',
    ...     'False  False  False',
    ...     'True   False  False',
    ...     'False  True   False',
    ...     'True   True   True ',
    ...     '=====  =====  =====',
    ... ]
    True

.. _reStructuredText: http://docutils.sourceforge.net/rst.html

tsv
===

A variant of the csv_ format with tabulators as fields separators.

xls
===

Import/export data in Legacy Excel Spreadsheet representation.

This format is optional, install Tablib with ``pip install tablib[xls]`` to
make the format available.

.. note::

    XLS files are limited to a maximum of 65,000 rows. Use xlsx_ to avoid this
    limitation.

.. admonition:: Binary Warning

    The ``xls`` file format is binary, so make sure to write in binary mode::

        with open('output.xls', 'wb') as f:
            f.write(data.export('xls'))

xlsx
====

Import/export data in Excel 07+ Spreadsheet representation.

This format is optional, install Tablib with ``pip install tablib[xlsx]`` to
make the format available.

.. admonition:: Binary Warning

    The ``xlsx`` file format is binary, so make sure to write in binary mode::

        with open('output.xlsx', 'wb') as f:
            f.write(data.export('xlsx'))

yaml
====

Import/export data in the YAML_ format.
When exporting, if headers have been set, a YAML list of objects will be
returned. If no headers have been set, a YAML list of lists (rows) will be
returned instead.

Import assumes (for now) that headers exist.

This format is optional, install Tablib with ``pip install tablib[yaml]`` to
make the format available.

.. _YAML: https://yaml.org
