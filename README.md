# utl-self-join-get-all-combinations-where-left-col-less-then-right-col-sql-wps-r-and-python
Self join select all combinations where left col is less then right col in sql wps r and python
 %let pgm=utl-self-join-get-all-combinations-where-left-col-less-then-right-col-sql-wps-r-and-python;

    Self join select all combinations where left col is less then right col in sql wps r and python

    Assumes that within a date the ID is sorted (easy to remove this restriction)

    github
    https://tinyurl.com/3nfwfacz
    https://github.com/rogerjdeangelis/utl-self-join-get-all-combinations-where-left-col-less-then-right-col-sql-wps-r-and-python


    stackoverflow r
    https://tinyurl.com/53d48y3s
    https://stackoverflow.com/questions/77587543/separate-1-column-into-2-based-on-occurrences-on-the-same-day-in-r

      Solutions

          1 wps sql
          2 wps r sql
          3 wps python sql
          4 wps r native

    SOAPBOX ON
      I suspect as python matures it will enhance pyyhon sq functionality and performance.
    SOAPBOX OFF

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                          |                     |                                                                       */
    /*      INPUT               |    STEP 1           |    OUTPUT                                                             */
    /*                          |                     |                                                                       */
    /* SD1.HAVE total obs=30    |   REMOVE DUPS       |  FORM COMBINATIONS                                                    */
    /*                          |                     |                                                                       */
    /* bs       DATE       ID   |      DATE       ID  |      DATE       id1    id2                                            */
    /*                          |                     |                                                                       */
    /*  1    2010-01-02    A    |   2010-01-02    A   |   2010-01-02                                                          */
    /*  2    2010-01-02    A    |   2010-01-02    W   |   2010-01-02     A      W                                             */
    /*  3    2010-01-02    A    |                     |                                                                       */
    /*  4    2010-01-02    A    |   2010-01-11    A   |   2010-01-11     A      G                                             */
    /*  5    2010-01-02    A    |   2010-01-11    G   |   2010-01-11     A      K                                             */
    /*  6    2010-01-02    A    |   2010-01-11    K   |   2010-01-11     A      W                                             */
    /*  7    2010-01-02    A    |   2010-01-11    W   |                                                                       */
    /*  8    2010-01-02    A    |                     |   2010-01-11     G      K                                             */
    /*  9    2010-01-02    A    |                     |   2010-01-11     G      W                                             */
    /* 10    2010-01-02    W    |                     |                                                                       */
    /* 11    2010-01-02    W    |                     |   2010-01-11     K      W                                             */
    /* 12    2010-01-02    W    |                     |                                                                       */
    /* 13    2010-01-02    W    |                     |                                                                       */
    /* 14    2010-01-11    A    |                     |                                                                       */
    /* 15    2010-01-11    A    |                     |                                                                       */
    /* 16    2010-01-11    A    |                     |                                                                       */
    /* 17    2010-01-11    A    |                     |                                                                       */
    /* 18    2010-01-11    A    |                     |                                                                       */
    /* 19    2010-01-11    A    |                     |                                                                       */
    /* 20    2010-01-11    A    |                     |                                                                       */
    /* 21    2010-01-11    A    |                     |                                                                       */
    /* 22    2010-01-11    A    |                     |                                                                       */
    /* 23    2010-01-11    G    |                     |                                                                       */
    /* 24    2010-01-11    G    |                     |                                                                       */
    /* 25    2010-01-11    G    |                     |                                                                       */
    /* 26    2010-01-11    K    |                     |                                                                       */
    /* 27    2010-01-11    K    |                     |                                                                       */
    /* 28    2010-01-11    K    |                     |                                                                       */
    /* 29    2010-01-11    W    |                     |                                                                       */
    /* 30    2010-01-11    W    |                     |                                                                       */
    /*                          |                     |                                                                       */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    data sd1.have;
    input Date $11. ID $1.;
    cards4;
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 A
    2010-01-02 W
    2010-01-02 W
    2010-01-02 W
    2010-01-02 W
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 A
    2010-01-11 G
    2010-01-11 G
    2010-01-11 G
    2010-01-11 K
    2010-01-11 K
    2010-01-11 K
    2010-01-11 W
    2010-01-11 W
    ;;;;
    run;quit;

    /*                                  _
    / | __      ___ __  ___   ___  __ _| |
    | | \ \ /\ / / `_ \/ __| / __|/ _` | |
    | |  \ V  V /| |_) \__ \ \__ \ (_| | |
    |_|   \_/\_/ | .__/|___/ |___/\__, |_|
                 |_|                 |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    options validvarname=any;
    libname sd1 "d:/sd1";
    proc sql;
      create
         table sd1.want as
      select
       distinct
        l.date
        ,l.id  as id1
        ,r.id  as id2
      from
         sd1.have as l, sd1.have as r
      where
                l.id < r.id
          and l.date = r.date
    ;quit;

    proc print data=sd1.want;
    run;quit;
    ');

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*  Obs       DATE       id1    id2                                                                                       */
    /*                                                                                                                        */
    /*   1     2010-01-02     A      W                                                                                        */
    /*   2     2010-01-11     A      G                                                                                        */
    /*   3     2010-01-11     A      K                                                                                        */
    /*   4     2010-01-11     A      W                                                                                        */
    /*   5     2010-01-11     G      K                                                                                        */
    /*   6     2010-01-11     G      W                                                                                        */
    /*   7     2010-01-11     K      W                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                          _
    |___ \  __      ___ __  ___   _ __   ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| | `__| / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ | |    \__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |_|    |___/\__, |_|
                     |_|                        |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    libname sd1 "d:/sd1";

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);
    want<-sqldf("
      select
       distinct
        l.date
        ,l.id  as id1
        ,r.id  as id2
      from
         have as l, have as r
      where
                l.id < r.id
          and l.date = r.date
    ");
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    ');

    proc print data=sd1.want width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS R System                WPS System                                                                            */
    /*                                                                                                                        */
    /*          DATE id1 id2         DATE       ID1    ID2                                                                    */
    /*                                                                                                                        */
    /*  1 2010-01-02   A   W      2010-01-02     A      W                                                                     */
    /*  2 2010-01-11   A   G      2010-01-11     A      G                                                                     */
    /*  3 2010-01-11   A   K      2010-01-11     A      K                                                                     */
    /*  4 2010-01-11   A   W      2010-01-11     A      W                                                                     */
    /*  5 2010-01-11   G   K      2010-01-11     G      K                                                                     */
    /*  6 2010-01-11   G   W      2010-01-11     G      W                                                                     */
    /*  7 2010-01-11   K   W      2010-01-11     K      W                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                   _   _                             _
    |___ / __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \ \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/   \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
                    |_|         |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    libname sd1 "d:/sd1";

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;
    import pyreadstat as pr;
    from os import path;
    import pandas as pd;
    import numpy as np;
    from pandasql import sqldf;
    mysql = lambda q: sqldf(q, globals());
    from pandasql import PandaSQL;
    pdsql = PandaSQL(persist=True);
    sqlite3conn = next(pdsql.conn.gen).connection.connection;
    sqlite3conn.enable_load_extension(True);
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll');
    mysql = lambda q: sqldf(q, globals());
    want = pdsql('''
      select
       distinct
        l.date
        ,l.id  as id1
        ,r.id  as id2
      from
         have as l, have as r
      where
                l.id < r.id
          and l.date = r.date
    ''');
    print(want);
    pr.write_xport(want, 'd:/xpt/want.xpt',table_name='want',file_format_version=5);
    endsubmit;
    run;quit;
    ");

    libname xpt xport "d:/xpt/want.xpt";
    proc conten ts data=xpt._all_;
    run;quit;
    data want;
      set xpt.want;
    run;quit;
    proc print;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS PYTHON Procedure                                                                                              */
    /*                                                                                                                        */
    /*            DATE id1 id2                                                                                                */
    /*  0  2010-01-02    A   W                                                                                                */
    /*  1  2010-01-11    A   G                                                                                                */
    /*  2  2010-01-11    A   K                                                                                                */
    /*  3  2010-01-11    A   W                                                                                                */
    /*  4  2010-01-11    G   K                                                                                                */
    /*  5  2010-01-11    G   W                                                                                                */
    /*  6  2010-01-11    K   W                                                                                                */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                             _   _
    | || |   __      ___ __  ___   _ __   _ __   __ _| |_(_)_   _____
    | || |_  \ \ /\ / / `_ \/ __| | `__| | `_ \ / _` | __| \ \ / / _ \
    |__   _|  \ V  V /| |_) \__ \ | |    | | | | (_| | |_| |\ V /  __/
       |_|     \_/\_/ | .__/|___/ |_|    |_| |_|\__,_|\__|_| \_/ \___|
                      |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
    library(dplyr);
    want <- have %>%
      reframe(
        setNames(data.frame(t(combn(unique(ID), 2))), c('FROM', 'TO')),
        .by = 'DATE'
      );
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    ");

    proc print data=sd1.want;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /*          DATE FROM TO                                                                                                  */
    /*                                                                                                                        */
    /*  1 2010-01-02    A  W                                                                                                  */
    /*  2 2010-01-11    A  G                                                                                                  */
    /*  3 2010-01-11    A  K                                                                                                  */
    /*  4 2010-01-11    A  W                                                                                                  */
    /*  5 2010-01-11    G  K                                                                                                  */
    /*  6 2010-01-11    G  W                                                                                                  */
    /*  7 2010-01-11    K  W                                                                                                  */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* STEPS:                                                                                                                 */
    /*                                                                                                                        */
    /*  .by="Date" is one of dplyr's ways to do grouped ops "once"                                                            */
    /*  because combn returns a matrix, we need to transpose it and frame it,                                                 */
    /*  with data.frame(t(...)); since the default names are "V1" and "V2",                                                   */
    /*  we wrap that in setNames to get the names you want                                                                    */
    /*  a lesser-known functionality of dplyr's data verbs (e.g., mutate, summarize, reframe)                                 */
    /*  is that if the inner expression returns a frame, it will be appended (cbind-style)                                    */
    /*  to the existing frame for mutate, and replace it (with groups) with summarize/reframe,                                */
    /*  names and all. (This is one reason why I wrapped in data.frame(.).)                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */





















