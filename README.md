MDB2MySQL README - May 14, 2015
====================================

Created By: Bill Lewis - Enobis (www.enobis.com)

Latest Version: v1.0.3
Available At: http://github.com/rogerswb/mdb2mysql

This is free software; you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation - visit: http://www.gnu.org/copyleft/gpl.html

INTRODUCTION
------------------
Plain and simple, this program was created to produce an SQL format
comparable to mysqldump for converting Microsoft Access databases to
MySQL.

MOTIVATION
------------------
MDB2MySQL was originally created for internal use for converting our client's
Microsoft Access (.mdb) files to a form suitable for porting to the MySQL
relational database.  The program is a Perl script, written to work in
conjunction with MDB Tools (mdbtools.sourceforge.net), by taking the
information contained within the MDB file and converting it into a format
similar to that obtained by dumping a MySQL database.

  The output of MDB2MySQL is written to standard out and can be either
redirected to a file, or piped directly into MySQL using the mysql client.
The script is capable of exporting table schema, data with suitable inserts,
and a combination of both.  All Microsoft Access file types are converted
into corresponding, suitable, MySQL field types.

  Doesn't MDB Tools already do this?  Well, yes and no.  While MDB Tools is
an awesome tool for extracting information from MDB files, the info still
needed to be heavily manipulated before it was capable for importing into
MySQL.  In particular, the field types needed to be converted to suitable
MySQL field types.  Given that our client's databases often had numerous
tables and database sizes of 50 to upwards of 100 MBytes, this made the task
of hand manipulation daunting.  Thus we looked to scripting the process, and
MDB2MySQL was born.

REQUIREMENTS
------------------
Given that MDB2MySQL is a Perl script, the first obvious requirement is Perl.
Which can be obtained at:

http://www.perl.com/ or http://www.cpan.org/

Secondly, the script requires MDB Tools version 0.7.1, or later, for extracting the
information from the MDB file, which can be obtained from:

http://github.com/brianb/mdbtools/

This program has been tested on Linux machines, using Perl v5.8.4.  I
suspect the limitation here is going to be MDB Tools, so if you can build
MDB Tools on a different flavor of Unix, then I suspect MDB2MySQL should
work without difficulty.

USAGE
------------------
Currently there are no man pages for the program, and not sure there will be
anytime soon, so consider this file the manual for the time being.  Most of
the options should be self explanatory, with a few exceptions.

Usage: mdb2mysql [options] <mdb file>
  -c             Create table structure only, no data.
  -d             Add a 'drop table' before each create.
  -e             Use the much faster, extended INSERT syntax.
  -i             Export data inserts only.
  -l             Add locks around insert statements.
  -o <tables>    Omit tables in this comma seperated list.
  -r <character> Replace illegal characters with given character.
                 The default character is an underscore.
  -t <tables>    Export only this list of comma seperated tables.
  -u             Report unknown Access data type and exit.
  -x             Same as using -d -e -l combined options.
  -U <type>      Use the MySQL data type for unknown Access types.
                 Unless given, 'blob' will be used by default.
  -h, --help     This message and exit.
  -V, --version  Output version information and exit.

Further Explanations:

  -r <character>
     Microsoft Access allows characters other than alphanumeric ones,
     including spaces, to be used as field names, which are illegal in
     MySQL.  This option allows you to change the default underscore that
     is used in replacement of these illegal characters, to a character of
     your choosing.  Please refer to the MySQL manual for a list of legal
     characters to be used for field names.

  -x
     This option produces a format similar to using the --opt option with
     mysqldump.

This program has been successfully used to convert MDB files for both the
JET3 (Access 97) and JET4 (Access 2000 & XP) versions, as well as files as
small as 500 KB to as large as 125 MB.  While not tested, larger files should
work as well.

DATA TYPES
------------------
The following is a list of the known Access Data Types and the corresponding
MySQL data type used for conversion by the program:

  Access Data Type      MySQL Data Type
  --------------------  --------------------
  Text                  if less than 2 characters a char otherwise a varchar
                        of corresponding length.
  Memo/Hyperlink        text
  Byte                  tinyint
  Integer               smallint
  LongInteger           int
  Single                double
  Double                double
  Numeric               float
  Currency              decimal(10,2)
  DateTime              datetime
  Boolean               enum('1','0')
  ReplicationID         tinyblob
  OLE                   longblob

Any unknown Access data type will be converted to a blob, unless indicated
otherwise by using the -U option.

BUGS & ISSUES
------------------
There are currently no known bugs or issues, however, that doesn't mean there
aren't any.  If you find any, please report them.

LEGAL STUFF
------------------
This program was written by Bill Lewis and is copyrighted (C) 2004.  You may
do whatever you like with the source code, provided that you continue to
acknowledge the author in any distribution of it, and of any distribution of
code substantially derived from it; and that you make the original source code
available.  Also, please include this README file along with any distribution.
Needless to say, there is no warranty of any kind, and you use this at your
own risk.
