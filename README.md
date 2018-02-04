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

