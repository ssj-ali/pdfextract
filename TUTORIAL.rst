Tutorial for template creation
==============================

A template defines which data attributes you wish to retrieve from an
invoice. Each template should work on all invoices of a company or
subsidiary (e.g. Amazon Germany vs Amazon US).

Adding templates is easy and shouldn’t take longer than adding 2-3
invoices by hand. We use a simple YML-format. Many options are optional
and you just need them to take care of edge cases.

Existing templates can be found in the project folder or the installed
package under ``/pakage/mytemplates/``.

Simple invoice template
-----------------------

Here is a sample of a minimal invoice template to read invoiced issued
by Microsoft Hong Kong:

::

    issuer: Korean Air
    fields:
        RefNo: Order\s+Number:\s+(\w+)
        PartNumber: Part\s+Number\s+(\w+-\w+)
        SerialNumber: Serial Number\s+(?=[A-Z])(\w+)
        PartDescription: Description\s+((?:\S ?)+)
        DeliveryDate: Removal Date\s+(\d+-\w+-\d+)
        CustomerName: Name\s+((?:\S+ ?)+)
    keywords:
    - Korea
    - Seoul


Let’s look at each field:

-  ``issuer``: The name of the invoice issuer. Can have the company name
   and country.
-  ``keywords``: Also a required field. These are used to pick the
   correct template. Be as specific as possible. As we add more
   templates, we need to avoid duplicate matches.

Fields
~~~~~~

All the regex ``fields`` you need extracted. Required fields are
``SerialNumber``, ``DeliveryDate``, ``POnumber``. It’s up to you, if you need
more fields extracted. Each field has one or more regex with one regex
capturing group. It’s not required to put add the whole regex to the
capturing group. Often we use keywords and only capture part of the
match.

You will need to understand regular expressions to find the right
values. See about them
`here <http://www.zytrax.com/tech/web/regex.htm>`__ or `test them
here <http://www.regexr.com/>`__. We use `Python’s regex
engine <https://docs.python.org/2/library/re.html>`__. It won’t matter
for the simple expressions we need, but sometimes there are subtle
differences when e.g. coming from Perl.


Lines
~~~~~

The ``lines`` key allows you to parse invoice items. Mandatory are
regexes ``start`` and ``end`` to figure out where in the stream the item
table is located. Then the regex ``line`` is applied, and supposed to
contain named capture groups. The names of the capture groups will be
the field names for the parsed item. If we have an invoice that looks
like

::

    some header text

    the address, etc.

      Item        Qty             Price

     1st item     0               00.00
     2nd item     0               00.00

                          Total   00.00

    A footer

your lines definition should look like

::

    lines:
        start: Item\s+Qty\s+Price$
        end: \s+Total
        line: (?P<description>.+)\s+(?P<Qty>\d+.\d+)\s+(?P<price>\d+\d+)



Tables
~~~~~

The ``tables``  allows you to parse table-oriented fields that have a row
of column headers followed by a row of values on the next line. The plugin
requires a ``start`` and ``end`` regex to identify where the table is located
in the invoice. The ``body`` regex should contain named capture groups that
will be added to the fields output. The plugin will attempt to match the
``body`` regex to the invoice content found between the ``start`` and ``end``
regexes.

An example invoice that contains table-oriented data may look like:

::

    Company Name: American Airlines                                                         RO No. 8666857579

    Part Details                                                       P/N:                S/N:        Batch
    PCBA,SDB,PROC,NAN-DR.                                             101-AJ878          363574348     733374
    
                                                                        Delivery Date               Contact
                                                                        22-May-2019                Fred Lee  

Shipping Instruction: - If the chargeable weight (volumetric weight or actual weight whichever is higher) of the shipment is
non hazmat and 40Kgs, please use DHL Express account# 963291042 and if > 40Kgs and/or hazmat, please contact 24/7
Ethiopian Logistics at ETInboundlogistics@ethiopianairlines.com <mailto:ETInboundlogistics@ethiopianairlines.com>
Tel: +251-929-908-759 or +251-929-908-757 for a shipping instruction.
Shipments should be onboard Ethiopian fleet only and delivered to Ethiopian cargo office or Ethiopian forwarding agent.


The PartDescription, PartNumber and SerialNumber, Batch, deliverydate, and
Contact are all located on different lines from their column headings.
A template to capture these fields may look like:

::

    tables:
      - start: Part Details\s+P/N\s+S/N\s+Batch
        end: Delivery Date
        body: (?P<PartDescription>(?:\S ?)+)\s+(?P<PartNumber>\w+-\w+)\s+(?P<SerialNumber>\w+)\s+(?P<Batch>\d+)
      - start: Shipping Date\s+Contact
        end: DESCRIPTION
        body: (?P<DeliveryDate>\d{1,2}-\w+-\d{4})\s+(?P<Contact>(?:\w+ ?)+)




Steps to add new template
-------------------------

To add a new template, I recommend this workflow:

1. Copy existing template to new file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Find a template that is roughly similar to what you need and copy it to
a new file. 

2. Change invoice issuer
~~~~~~~~~~~~~~~~~~~~~~~~

Just used in the output. Best to use the company name.

3. Set keyword
~~~~~~~~~~~~~~

Look at the invoice and find the best identifying string. Location +
company name are good options. Remember, *all* keywords need to be found
for the template to be used.


4. First test run
~~~~~~~~~~~~~~~~~

Now open that th ``main.exe`` application click on ''Extract text'' button.
Then select the pdf of which you want to make the template of.
So instead of using PDF directly, one should extract its data and then use it 
to make template. This is preferred because PDF data are not well formatted so 
there can be difference between what is seen & what is extracted. 

5. Add regular expressions
~~~~~~~~~~~~~~~~~~~~~~~~~~

Now you can use add regex fields for the information you need. 

6. Done
~~~~~~~

Now you’re ready to push your template. See other example of pdf and their
templates in the zip file attached.
