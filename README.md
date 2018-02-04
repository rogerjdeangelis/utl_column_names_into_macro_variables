# utl_column_names_into_macro_variables
Column names into macro variables. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Column names into macro variables

      Two Solutions
          1. With utl_varGet macro
          2. Without a macro

    Original topic: Parse all column names from table and put into macro variable (SAS 9.4)

    github
    https://github.com/rogerjdeangelis/utl_column_names_into_macro_variables

    Macros on end
       %utlopts
       %utlopts;

    see
    https://goo.gl/3bwyw5
    https://stackoverflow.com/questions/48580934/parse-all-column-names-from-table-and-put-into-macro-variable-sas-9-4


    INPUT
    =====

      SASHELP.CLASS total obs=19

        NAME       SEX    AGE    HEIGHT    WEIGHT

        Alfred      M      14     69.0      112.5
        Alice       F      13     56.5       84.0
        Barbara     F      13     65.3       98.0
        Carol       F      14     62.8      102.5
        Henry       M      14     63.5      102.5
        James       M      12     57.3       83.0
        ...

      RULES Create global macro variable)
      ===================================

        AGE     = NA
        HEIGHT  = NA
        NAME    = NA
        SEX     = NA
        WEIGHT  = NA


    PROCESS (All the code)
    ======================

      1. WITH UTL_VARGET MACRO(all the code)

        * not needed;
        %symdel name sex age height weight; * just in case you rerun;

        %utlnopts;

        %macro utl_varGet(dsn) / des="Put variable names into macro variable";
            %let dsid=%sysfunc(open(&dsn,is));
            %let nvars=%sysfunc(attrn(&dsid,nvars));
            %do i=1 %to &nvars;
              %global %sysfunc(varname(&dsid,&i));
              %let %sysfunc(varname(&dsid,&i))=NA;
              %put %sysfunc(varname(&dsid,&i));
            %end;
            %let rc=%sysfunc(close(&dsid));
        %mend utl_varGet;

        %utl_varGet(sashelp.class);

        * SAS may clobber your USER symbol table with SAS generated
          'SYS' hidden 'USER?' macro variables;

        %put _user_;

           GLOBAL AGE     NA
           GLOBAL HEIGHT  NA
           GLOBAL NAME    NA
           GLOBAL SEX     NA
           GLOBAL WEIGHT  NA

        %utlopts;


      2. WITHOUT A MACRO(all the code)

         %symdel name sex age height weight; * just in case you rerun;

         %let dsid = %sysfunc(open(sashelp.class(obs=1),i));
         %syscall set(dsid);
         %let rc   =%sysfunc(fetchobs(&dsid,1));
         %let rc   =%sysfunc(close(&dsid));

         %symdel dsid rc / nowarn;

         %put _user_;

         %put
            &=NAME
            &=SEX
            &=AGE
            &=HEIGHT
            &=WEIGHT
            ;

         NAME        = Alice
         SEX         = F
         AGE         = 13
         HEIGHT      = 56.5
         WEIGHT      = 84


    OUTPUT
    ======

       1. WITH UTL_VARGET MACRO (because of utlnopts setting)

         NAME
         SEX
         AGE
         HEIGHT
         WEIGHT

        %put _user_;

         GLOBAL AGE     NA
         GLOBAL HEIGHT  NA
         GLOBAL NAME    NA
         GLOBAL SEX     NA
         GLOBAL WEIGHT  NA

       2. WITHOUT A MACRO

         NAME        = Alice
         SEX         = F
         AGE         = 13
         HEIGHT      = 56.5
         WEIGHT      = 84

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

     just use sashelp.class

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    * same as above;

    1. WITH UTL_VARGET MACRO

    * not needed;
    %symdel name sex age height weight; * just in case you rerun;

    %utlnopts;

    %macro utl_varGet(dsn) / des="Put variable names into blak separated string";
        %let dsid=%sysfunc(open(&dsn,is));
        %let nvars=%sysfunc(attrn(&dsid,nvars));
        %do i=1 %to &nvars;
          %global %sysfunc(varname(&dsid,&i));
          %let %sysfunc(varname(&dsid,&i))=NA;
          %put %sysfunc(varname(&dsid,&i));
        %end;
        %let rc=%sysfunc(close(&dsid));
    %mend utl_varGet;

    %utl_varGet(sashelp.class);


    2. WITHOUT A MACRO

    %symdel name sex age height weight; * just in case you rerun;

    %let dsid = %sysfunc(open(sashelp.class(obs=1),i));
    %syscall set(dsid);
    %let rc   =%sysfunc(fetchobs(&dsid,1));
    %let rc   =%sysfunc(close(&dsid));

    %symdel dsid rc / nowarn;

    %put _user_;


    %macro utlnopts(note2err=nonote2err,nonotes=nonotes)
        / des = "Turn  debugging options off";

    OPTIONS
         FIRSTOBS=1
         NONUMBER
         MLOGICNEST
       /*  MCOMPILENOTE */
         MPRINTNEST
         lrecl=384
         MAUTOLOCDISPLAY
         NOFMTERR     /* turn  Format Error off                           */
         NOMACROGEN   /* turn  MACROGENERATON off                         */
         NOSYMBOLGEN  /* turn  SYMBOLGENERATION off                       */
         &NONOTES     /* turn  NOTES off                                  */
         NOOVP        /* never overstike                                  */
         NOCMDMAC     /* turn  CMDMAC command macros on                   */
         NOSOURCE    /* turn  source off * are you sure?                 */
         NOSOURCE2    /* turn  SOURCE2   show gererated source off        */
         NOMLOGIC     /* turn  MLOGIC    macro logic off                  */
         NOMPRINT     /* turn  MPRINT    macro statements off             */
         NOCENTER     /* turn  NOCENTER  I do not like centering          */
         NOMTRACE     /* turn  MTRACE    macro tracing                    */
         NOSERROR     /* turn  SERROR    show unresolved macro refs       */
         NOMERROR     /* turn  MERROR    show macro errors                */
         OBS=MAX      /* turn  max obs on                                 */
         NOFULLSTIMER /* turn  FULLSTIMER  give me all space/time stats   */
         NODATE       /* turn  NODATE      suppress date                  */
         DSOPTIONS=&NOTE2ERR
         ERRORCHECK=STRICT /*  syntax-check mode when an error occurs in a LIBNAME or FILENAME statement */
         DKRICOND=ERROR    /*  variable is missing from input data during a DROP=, KEEP=, or RENAME=     */

         /* NO$SYNTAXCHECK  be careful with this one */
    ;

    RUN;quit;

    %MEND UTLNOPTS;

    %MACRO UTLOPTS
             / des = "Turn all debugging options off forgiving options";

    OPTIONS

       OBS=MAX
       FIRSTOBS=1
       lrecl=384
       NOFMTERR      /* DO NOT FAIL ON MISSING FORMATS                              */
       SOURCE      /* turn sas source statements on                               */
       SOURCe2     /* turn sas source statements on                               */
       MACROGEN    /* turn  MACROGENERATON ON                                     */
       SYMBOLGEN   /* turn  SYMBOLGENERATION ON                                   */
       NOTES       /* turn  NOTES ON                                              */
       NOOVP       /* never overstike                                             */
       CMDMAC      /* turn  CMDMAC command macros on                              */
       /* ERRORS=2    turn  ERRORS=2  max of two errors                           */
       MLOGIC      /* turn  MLOGIC    macro logic                                 */
       MPRINT      /* turn  MPRINT    macro statements                            */
       MRECALL     /* turn  MRECALL   always recall                               */
       MERROR      /* turn  MERROR    show macro errors                           */
       NOCENTER    /* turn  NOCENTER  I do not like centering                     */
       DETAILS     /* turn  DETAILS   show details in dir window                  */
       SERROR      /* turn  SERROR    show unresolved macro refs                  */
       NONUMBER    /* turn  NONUMBER  do not number pages                         */
       FULLSTIMER  /*   turn  FULLSTIMER  give me all space/time stats            */
       NODATE      /* turn  NODATE      suppress date                             */
       /*DSOPTIONS=NOTE2ERR                                                                              */
       /*ERRORCHECK=STRICT /*  syntax-check mode when an error occurs in a LIBNAME or FILENAME statement */
       DKRICOND=WARN      /*  variable is missing from input data during a DROP=, KEEP=, or RENAME=     */
       DKROCOND=WARN      /*  variable is missing from output data during a DROP=, KEEP=, or RENAME=     */
       /* NO$SYNTAXCHECK  be careful with this one */
     ;

    run;quit;

    %MEND UTLOPTS;

