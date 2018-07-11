# utl_proc_report_page_x_of_y_by_group
Proc report page x of y by group.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.


    Proc report page x of y by group

    see github
    https://tinyurl.com/ycg4bz3j
    https://github.com/rogerjdeangelis/utl_proc_report_page_x_of_y_by_group

    SAS Forum
    https://communities.sas.com/t5/SAS-Procedures/Page-X-of-Y/m-p/476978

    for other report tips
    https://tinyurl.com/ycnbdamo
    https://github.com/rogerjdeangelis?utf8=%E2%9C%93&tab=repositories&q=report&type=&language=


    INPUT
    =====

       sashelp.baseball

       Rules

       Construct Page X of Y by group

       For the American League we have

       Title "American League
       Page 1 of 4
       Page 2 of 4
       Page 3 of 4
       Page 4 of 4

       For the National Leage  we have

       Title "National League
       Page 1 of 3
       Page 2 of 3
       Page 3 of 3



      EXAMPLE OUTPUT
      --------------

       Page 1 of 4

       American Baseball Home Run Hitters      *** Title only appears on first page of each leage

         League    Division
         at the    at the                                             Home
         End of    End of    Team at the                           Runs in
         1986      1986      End of 1986     Player's Name            1986
         American  East      Cleveland       Allanson, Andy              1
         American  West      Seattle         Davis, Alan                18
         American  West      Oakland         Griffin, Alfredo            4
         American  West      Kansas City     Salazar, Argenis            0
         American  East      Cleveland       Thornton, Andre            17

        ...

       Page 4 of 4

         League    Division
         at the    at the                                             Home
         End of    End of    Team at the                           Runs in
         1986      1986      End of 1986     Player's Name            1986
         American  West      Minneapolis     Smalley, Roy               20
         American  West      California      Wilfong, Rob                3
         American  East      Milwaukee       Yount, Robin                9
         American  West      Kansas City     Balboni, Steve             29
         American  West      Seattle         Bradley, Scott              5
         American  West      Texas           Buechele, Steve            18


    PROCESS
    =======

    * the whole purpose of all this datastep is to populate
    the page number and calculate the number of pages;

    data havRpt;

     if _n_=0 then do;
        %let rc=%sysfunc(dosubl('
           proc sort data=sashelp.baseball(keep=league division team name nhome) out=havSrt;
             by league;
           run;quit;
        '));
     end;

     retain lyns lyn 0 pge 1;
     set havSrt;
     by league;

     lyn=lyn+1;
     lyns=mod(lyn,50);

     if lyns=0 then pge=pge+1;
     output;
     if last.league then do;
         call symputx(league,pge);
         pge=1;
         lyn=0;
     end;
     drop lyn lyns;

    run;quit;

    /* There is a trick here
      Maybe clearer to use two macro variables?
      Max pages

    * &&&leag = &America = 4
    * &leag = American;
    */

    %macro rpt(leag);

      %do pge=1 %to &&&leag;
        %if &pge=1 %then %do;
            title1 "Page &pge of &&&leag" ;
            title2 "&leag Baseball Home Run Hitters";
        %end;
        %else %do;
            title1 "Page &pge of &&&leag" ;
            title2;
        %end;
        proc report data=havPge(where=(pge=&pge and league="&leag")) nowd missing;
        cols league division team name nhome;
        run;quit;
      %end;

    %mend rpt;

    %rpt(American)
    %rpt(National)


    OUTPUT
    ======

    Page 1 of 4
    American Baseball Home Run Hitters

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      American  East      Cleveland       Allanson, Andy              1
      American  West      Seattle         Davis, Alan                18

    Page 2 of 4

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      American  East      New York        Pasqua, Dan                16
      American  West      Texas           Porter, Darrell            12


    Page 3 of 4

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      American  East      Baltimore       Traber, Jim                13
      American  East      Detroit         Gibson, Kirk               28

    Page 4 of 4

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      American  West      Minneapolis     Smalley, Roy               20
      American  West      California      Wilfong, Rob                3





    Page 1 of 3
    National Baseball Home Run Hitters

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      National  West      Houston         Ashby, Alan                 7
      National  East      Montreal        Dawson, Andre              20


    Page 2 of 3

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      National  West      Atlanta         Hubbard, Glenn              4
      National  East      Chicago         Matthews, Gary             21


    Page 3 of 3

      League    Division
      at the    at the                                             Home
      End of    End of    Team at the                           Runs in
      1986      1986      End of 1986     Player's Name            1986
      National  East      New York        Wilson, Mookie              9
      National  West      San Diego       Wynne, Marvell              7


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

      just use sashelp.baseball


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process

