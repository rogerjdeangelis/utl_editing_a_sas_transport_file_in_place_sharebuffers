# utl_editing_a_sas_transport_file_in_place_sharebuffers
Editing a sas transport file in place using sharebuffers.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

Editing a sas transport file in place using sharebuffers

Note CPORT and EXPORT SAS files have 80 byte boundaries?

I have a bunch of export files which create the same output SAS dataset.
I would like to edit the SAS dataset name in the export file.


see SAS forum
https://tinyurl.com/yark4rdu
https://communities.sas.com/t5/General-SAS-Programming/Converting-XPT-to-SAS-and-change-dataset-name/m-p/463085

Hex dum macro on end.

INPUT   (d:/xpt/class.xpt - has CLASS dataset)
=====

I want to change the name in the export file to ROGER

Note the sixth 80 byte record has the dataset name, CLASS.
Also note the 80 byte header record layout.

ASCII Flatfile Ruler & Hex
utlrulr
d:/xpt/class.xpt


 --- Record Number ---  1   ---  Record Length ---- 80

HEADER RECORD*******LIBRARY HEADER RECORD!!!!!!!000000000000000000000000000000
1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...8
44444525444542222222444545524444452544454222222233333333333333333333333333333322
8514520253F24AAAAAAAC92212908514520253F24111111100000000000000000000000000000000


 --- Record Number ---  2   ---  Record Length ---- 80

SAS     SAS     SASLIB  9.4     X64_7PRO                        17MAY18:15:01:20
1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...8
54522222545222225454442232322222533535542222222222222222222222223344533333333333
3130000031300000313C92009E400000864F702F00000000000000000000000017D1918A15A01A20


 --- Record Number ---  3   ---  Record Length ---- 80

17MAY18:15:01:20
1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...8
33445333333333332222222222222222222222222222222222222222222222222222222222222222
17D1918A15A01A200000000000000000000000000000000000000000000000000000000000000000


 --- Record Number ---  4   ---  Record Length ---- 80

HEADER RECORD*******MEMBER  HEADER RECORD!!!!!!!000000000000000001600000000140
1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...8
44444525444542222222444445224444452544454222222233333333333333333333333333333322
8514520253F24AAAAAAAD5D252008514520253F24111111100000000000000000160000000014000


 --- Record Number ---  5   ---  Record Length ---- 80

HEADER RECORD*******DSCRPTR HEADER RECORD!!!!!!!000000000000000000000000000000
1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...8
44444525444542222222454555524444452544454222222233333333333333333333333333333322
8514520253F24AAAAAAA433204208514520253F24111111100000000000000000000000000000000


 --- Record Number ---  6   ---  Record Length ---- 80

SAS     CLASS   SASDATA 9.4     X64_7PRO                        17MAY18:15:01:20
1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...8
54522222444552225454454232322222533535542222222222222222222222223344533333333333
313000003C133000313414109E400000864F702F00000000000000000000000017D1918A15A01A20



EXAMPLE OUTPUT

 Data Set Name        XPT.ROGER  ** was CLAS
 Member Type          DATA
 Engine               XPORT
 Created              05/17/2018 15:31:30
 Last Modified        05/17/2018 15:31:30

 Alphabetic List of Variables and Attributes

 #    Variable    Type    Len

 3    AGE         Num       8
 4    HEIGHT      Num       8
 1    NAME        Char      8
 2    SEX         Char      1
 5    WEIGHT      Num       8


PROCESS
=======

Change dataset name to ROGER

* reading card images;
filename xpt  "d:/xpt/class.xpt";
data _null_;
 infile xpt recfm=f lrecl=80 sharebuffers;
 file   xpt recfm=f lrecl=80;
 input;
 if _n_=6 then do;
   substr(_infile_,9,5)="ROGER";
   put;
 end;
run;
filename xpt clear;


OUTPUT
======

The CONTENTS Procedure

Data Set Name        XPT.ROGER                          Observations          .
Member Type          DATA                               Variables             5
Engine               XPORT                              Indexes               0
Created              05/17/2018 15:31:30                Observation Length    33
Last Modified        05/17/2018 15:31:30                Deleted Observations  0

Alphabetic List of Variables and Attributes

#    Variable    Type    Len

3    AGE         Num       8
4    HEIGHT      Num       8
1    NAME        Char      8
2    SEX         Char      1
5    WEIGHT      Num       8

*                _               _       _
 _ __ ___   __ _| | _____     __| | __ _| |_ __ _
| '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
| | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
|_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

;

libname xpt xport "d:/xpt/class.xpt";
data xpt.class;
 set sashelp.class;
run;quit;
libname xpt clear;

* HEX DUMP;
%utlrulr(
 uinflt=d:/xpt/class.xpt
,urecfm   = f
,ulrecl   = 80
,uobs = 10
,uprnlen=80
,uotflt=d:/txt/class.txt
);

*          _       _   _
 ___  ___ | |_   _| |_(_) ___  _ __
/ __|/ _ \| | | | | __| |/ _ \| '_ \
\__ \ (_) | | |_| | |_| | (_) | | | |
|___/\___/|_|\__,_|\__|_|\___/|_| |_|

;
* same as process;

* reading card images;
filename xpt  "d:/xpt/class.xpt";
data _null_;
 infile xpt recfm=f lrecl=80 sharebuffers;
 file   xpt recfm=f lrecl=80;
 input;
 if _n_=6 then do;
   substr(_infile_,9,5)="ROGER";
   put;
 end;
run;
filename xpt clear;

libname xpt xport "d:/xpt/class.xpt";
proc contents data=xpt._all_;
run;quit;
proc print data=xpt.ROGER;
run;quit;
libname xpt clear;


%macro utlrulr
      (
       utitle=ASCII Flatfile Ruler & Hex,
       uobj  =utlrulr,
       uinflt =c:\dat\delete.dat,
       /*--------------------------------*\
       | Control                          |
       \*--------------------------------*/
       uprnlen =70,  /* Linesize for Dump */
       ulrecl  =32760,  /* maximum record length */
       urecfm   =,
       uobs = 3,        /* number of obs to dump */
       uchrtyp =ascii,  /* ascii or ebcdic */
       /*--------------------------------*\
       | Outputs                          |
       \*--------------------------------*/
       uotflt =c:\dat\delete.hex
      ) / des = "ASCII Flatfile Ruler & Hex Chars";
    /*----------------------------------------------*\
    | Description:                                   |
    |  This code does a hex dump of a flatfile       |
    |  IPO                                           |
    |  ===                                           |
    |   INPUTS    UINFLT                             |
    |   ======                                       |
    |   A flatfile like                              |
    |    A2  NC 199607 JAN96   37   16    43  12     |
    |    A1  SC 199601 JAN91   17   26    45  32     |
    |    A2  ND 199602 JAN92   32   36    46         |
    |    A3  VT 199603 JAN93   47   54    47  35     |
    |    A4  NY 199604 JAN94   35   55    48  62     |
    |    A5  CA 199605 JAN95   57   56    49         |
    |    A6  NC 199606 JAN96   36   57    89  39     |
    |                                                |
    |   PROCESS                                      |
    |   =======                                      |
    |    Create a ruler line for each                |
    |    record. Echo the ASCII text.                |
    |    Print two lines with the hex                |
    |    representation.                             |
    |                                                |
    |   OUTPUT   UOTFLT    hex dump                  |
    |   ======                                       |
    |   Sample output                                |
    |    --- Record Number ---- 1                    |
    |    --- Record Length ---- 50                   |
    |                                                |
    |   A2  NC 199607 JAN96   37                     |
    |   1...5....10...15...20...2                    |
    |   4322442333333244433222332                    |
    |   1200E301996070A1E96000370                    |
    |                                                |
    |     16    43  12        567                    |
    |   5...30...35...40...45...5                    |
    |   2233222233223322222222333                    |
    |   0016000043001200000000567                    |
    |                                                |
    |    --- Record Number ---  2                    |
    |    --- Record Length ---- 39                   |
    |                                                |
    |   A1  SC 199601 JAN91   17                     |
    |   1...5....10...15...20...2                    |
    |   4322542333333244433222332                    |
    |   11003301996010A1E91000170                    |
    |     26    45  32                               |
    |   5...30...35...                               |
    |   22332222332233                               |
    |   00260000450032                               |
    |   Limitations                                  |
    |   1. Only handles records up to 1000 bytes     |
    |   2. Is very slow                              |
    \*----------------------------------------------*/
    /*--------------------------------------------------------------*\
    |  TESTCASE                                                      |
    |   data _null_;                                                 |
    |      file "c:\utl\delete.tst";                                 |
    |      put "A2  NC 199607 JAN96   37   16    43  12        567"; |
    |      put "A1  SC 199601 JAN91   17   26    45  32";            |
    |      put "A2  ND 199602 JAN92   32   36    46  42";            |
    |      put "A3  VT 199603 JAN93   47   54    47  35";            |
    |      put "A4  NY 199604 JAN94   35   55    48  62        789"; |
    |      put "A5  CA 199605 JAN95   57   56    49  72";            |
    |      put "A6  NC 199606 JAN96   36   57    89  39";            |
    |   run;                                                         |
    |   %utlrulr                                                     |
    |      (                                                         |
    |       uprnlen=25,                                              |
    |       uobs=3,                                                  |
    |       uinflt=c:\utl\delete.tst,                                |
    |       uotflt=c:\utl\delete.hex                                 |
    |      );                                                        |
    \*--------------------------------------------------------------*/
     options ls=80 ps=63;run;
     title1 "&utitle.";
     title2 "&uobj.";
     title3 "&uinflt.";
     title4 "&uotflt.";
     data _null_; file "&uotflt" print; put; run;
     title1;
       data _NULL_;
          length col $5. out $1;
          array uchr{1000} $1  u1-u1000;
          array urul{1000} $1  r1-r1000;
          infile "&uinflt" missover end=done length=ln lrecl=&ulrecl ignoredoseof
          %if "&urecfm"  ne  "" %then %str( recfm=&urecfm );
          ;
          file   "&uotflt" mod;
          input (uchr{*}) ( $char1.);
          recnum + 1;
          put #1 @;
          put #2 @;
          put #3 @uk ' --- Record Number ---  ' recnum '  ---  Record Length ---- ' ln  @;
          put #4 @;
          /*--------------------------------*\
          | Build the Ruler                  |
          \*--------------------------------*/
          urul{1}='1';
          urul{2}='.';
          urul{3}='.';
          urul{4}='.';
          do ui = 5 to 995 by 5;
             col  = compress( put ( ui, 4. ) !!'....' );
             do uj = 1 to 5;
                ul = ui + uj -1;
                urul{ul} = substr( col, uj, 1 );
             end;
          end;
          /*--------------------------------*\
          | Build Character & Hex Dump       |
          \*--------------------------------*/
          do ui = 1 to ln;
             uk+1;
             out = prxchange('s/[\x00-\x1F]/./', -1, uchr{ui});
             out = right(prxchange('s/[\x7F-\xFF]/./', -1, out));
             put #5 @uk out $char&uprnlen.. @;
             put #6 @uk urul{ui} $1. @;
             nibble1 = substr( put ( uchr{ui}, $hex2. ), 1, 1 );
             put #7 @uk nibble1 @;
             nibble2 = substr( put ( uchr{ui}, $hex2. ), 2, 1 );
             put #8 @uk nibble2 @;
             if mod( ui, &uprnlen ) eq 0 or  ( ln = ui )  then do;
                put ;
                uk=0;
             end;
          end;
          put ///;
          if recnum eq &uobs then stop;
      run;
   %mend utlrulr;


