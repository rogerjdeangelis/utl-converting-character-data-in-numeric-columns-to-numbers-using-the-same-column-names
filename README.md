# utl-converting-character-data-in-numeric-columns-to-numbers-using-the-same-column-names
Converting character data in numeric columns to numbers using the same column names and positions

    Converting character data in numeric columns to numbers using the same column names and positions

    github
    https://tinyurl.com/y37sb9la
    https://github.com/rogerjdeangelis/utl-converting-character-data-in-numeric-columns-to-numbers-using-the-same-column-names

    stackOverflow
    https://tinyurl.com/y4wsraum
    https://stackoverflow.com/questions/57016077/remove-invalid-string-entries-n-from-numeric-column-variables

    macros by Soren (and
    varlist:       Søren Lassen, s.lassen@post.tele.dk
    array do_over  Ted Clay, M.S.   tclay@ashlandhome.net  (541) 482-6435
                   David Katz, M.S. www.davidkatzconsulting.com
    Sister macros bdo_over abd barray (all datastep functionality) by
    Bartosz Jablonski
    yabwon@gmail.com

    github
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
      retain a1-a10;
      array numc[5] $2 a1 a3 a5 a7 a9;
      array numn[5] a2 a4 a6 a8 a10;
      do rec=1 to 6;
        do i=1 to dim(numc);
            if uniform(1234)<.2 then numc[i]="NA";
            else numc[i]=cats(int(10*uniform(1324)));
            numn[i]=int(10*uniform(1324));
        end;
      output;
      end;
      drop rec i;
    run;quit;

     Variables in Creation Order

     #    Variable    Type    Len

     1    A1          Char      2   Alternating character and numeric data
     2    A2          Num       8

     3    A3          Char      2
     4    A4          Num       8

     5    A5          Char      2
     6    A6          Num       8

     7    A7          Char      2
     8    A8          Num       8

     9    A9          Char      2
    10    A10         Num       8

    WORK.HAVE total obs=6
                                                                  |  RULES
      A1    A2    A3    A4    A5    A6    A7    A8    A9    A10   |
                                                                  |
      0      3    NA     2    NA     0    NA     4    NA     0    |  Convert NAs to missings
      0      9    7      4    3      5    3      2    1      7    |  Maintain order and same column names
      NA     5    2      7    NA     1    6      7    9      5    |
      4      9    3      1    1      3    5      3    5      6    |
      2      3    7      9    7      0    3      3    7      4    |
      NA     8    1      5    9      2    3      3    3      5    |
                                                                  |
                                                                  |
    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    Up to 40 obs from WANT total obs=6

      A1    A2    A3    A4    A5    A6    A7    A8    A9    A10

       0     3     .     2     .     0     .     4     .     0
       0     9     7     4     3     5     3     2     1     7
       .     5     2     7     .     1     6     7     9     5
       4     9     3     1     1     3     5     3     5     6
       2     3     7     9     7     0     3     3     7     4
       .     8     1     5     9     2     3     3     3     5

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %array(chrs,values=%utl_varlist(have,keep=_character_));
    %array(nums,values=%varlist(have,keep=_numeric_));

    proc sql;
      create
         table want as
      select
         %do_over(chrs nums,phrase=%str(input(?chrs,best12.) as ?chrs, ?nums),between=comma)
      from
        have
    ;quit;


