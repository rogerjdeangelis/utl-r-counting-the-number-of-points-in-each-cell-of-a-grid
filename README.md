# utl-r-counting-the-number-of-points-in-each-cell-of-a-grid
    %let pgm=utl-r-counting-the-number-of-points-in-each-cell-of-a-grid-AI;

    R counting the number of points in each cell of a grid AI

    There are four spacial packages in R.
    You can overlay the grids on a image and count points.
    Good for areal surveying (random samples)

        1. raster package:
        2. spatstat package:
        3. sfhotspot package:
        4. sf package:

    github
    https://tinyurl.com/ynjkc9y2
    https://github.com/rogerjdeangelis/utl-r-counting-the-number-of-points-in-each-cell-of-a-grid-AI

    Related repos on end


    /**************************************************************************************************************************/
    /*                                                        |                                          |                    */
    /*             INPUT                                      |                 PROCESS                  |    OUTPUT          */
    /*                                                        |                                          |                    */
    /*                                                        |                                          |                    */
    /* libname sd1 "d:/sd1";                                  | %utl_rbegin;                             |       x            */
    /* options validvarname=upcase;                           | parmcards4;                              |  y     [0,1) [1,2] */
    /*                                                        | library(spatstat)                        |  [1,2]     2     6 */
    /* data sd1.have;                                         | library(haven)                           |  [0,1)     3     7 */
    /*  do xx= 0 to 2;                                        | source("c:/temp/fn_tosas9.R")            |                    */
    /*    do yy= 0 to 2;                                      | have<-read_sas("d:/sd1/have.sas7bdat");  |                    */
    /*      do pts=1 to 2;                                    | pts <- ppp(have$X, have$Y,c(0,2),c(0,2)) |  SAS               */
    /*        x=2*uniform(3156);                              | want <- quadratcount(pts, nx=2, ny=2)    |                    */
    /*        y=2*uniform(6143);                              | want                                     |  V_0_1_    V_1_2_  */
    /*        ano=catx(',',round(x,.1),round(y,.1));          | fn_tosas9(dataf=want);                   |                    */
    /*        output;                                         | ;;;;                                     |     2         6    */
    /*      end;                                              | %utl_rend;                               |     3         7    */
    /*    end;                                                |                                          |                    */
    /*  end;                                                  | libname tmp "c:/temp";                   |                    */
    /* run;quit;                                              | proc print data=tmp.want(drop;           |                    */
    /*                                                        | run;quit;                                |                    */
    /* options ps=38 ls=64;                                   |                                          |                    */
    /* proc plot data=have;                                   |                                          |                    */
    /*  plot y*x= '*' $ ano / box                             |                                          |                    */
    /*    haxis=0 to 2 by 1                                   |                                          |                    */
    /*    vaxis=0 to 2 by 1                                   |                                          |                    */
    /*    vref=1                                              |                                          |                    */
    /*    href=1;                                             |                                          |                    */
    /* run;quit;                                              |                                          |                    */
    /*                                                        |                                          |                    */
    /*                              X                         |                                          |                    */
    /*    0                       1                       2   |                                          |                    */
    /*   -+-----------------------+-----------------------+-  |                                          |                    */
    /* 2 +                        |                        +2 |                                          |                    */
    /*   |    COUNT=2             |     COUNT=6            |  |                                          |                    */
    /*   |                  * 0.7,1.9                      |  |                                          |                    */
    /*   |                        |    1.3,1.7             |  |                                          |                    */
    /*   |                        1.3,1.7 * * * 1.4,1.7    |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |   * 1.2,1.6            |  |                                          |                    */
    /* Y |                        |                        |Y |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |    * 0.1,1.4           |                * 1.7,1.|  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |           * 1.5,1.2    |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /* 1 +------------------------+-------------*-1.6,1----+1 |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |    COUNT=3      * 0.7,0.8    COUNT=7            |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |            * 1.5,0.7   |  |                                          |                    */
    /*   |                        |           1.8,0.6 *    |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |  * 1.1,0.4             |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |                        |                        |  |                                          |                    */
    /*   |       * 0.3,0.2        |    1.3,0               |  |                                          |                    */
    /*   |               * 0.6,0.1|   *  *      1.9,0.1 *  |  |                                          |                    */
    /* 0 +                        |1.2,0.1                 +0 |                                          |                    */
    /*   -+-----------------------+-----------------------+-  |                                          |                    */
    /*    0                       1                       2   |                                          |                    */
    /*                            X                           |                                          |                    */
    /*                                                        |                                          |                    */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    libname sd1 "d:/sd1";
    options validvarname=upcase;

    data sd1.have;
     do xx= 0 to 2;
       do yy= 0 to 2;
         do pts=1 to 2;
           x=2*uniform(3156);
           y=2*uniform(6143);
           ano=catx(',',round(x,.1),round(y,.1));
           output;
         end;
       end;
     end;
    run;quit;

    options ps=38 ls=64;
    proc plot data=have;
     plot y*x= '*' $ ano / box
       haxis=0 to 2 by 1
       vaxis=0 to 2 by 1
       vref=1
       href=1;
    run;quit;

                                 X
       0                       1                       2
      -+-----------------------+-----------------------+-
    2 +                        |                        + 2
      |                        |                        |
      |                  * 0.7,1.9                      |
      |                        |    1.3,1.7             |
      |                      1.3,1.7 * * * 1.4,1.7      |
      |                        |                        |
      |                        |   * 1.2,1.6            |
    Y |                        |                        | Y
      |                        |                        |
      |    * 0.1,1.4           |                * 1.7,1.|
      |                        |                        |
      |                        |           * 1.5,1.2    |
      |                        |                        |
      |                        |                        |
    1 +------------------------+-------------*-1.6,1----+ 1
      |                        |                        |
      |                 * 0.7,0.8                       |
      |                        |                        |
      |                        |                        |
      |                        |            * 1.5,0.7   |
      |                        |           1.8,0.6 *    |
      |                        |                        |
      |                        |                        |
      |                        |  * 1.1,0.4             |
      |                        |                        |
      |                        |                        |
      |       * 0.3,0.2        |    1.3,0               |
      |               * 0.6,0.1|   *  *      1.9,0.1 *  |
    0 +                        |1.2,0.1                 + 0
      -+-----------------------+-----------------------+-
       0                       1                       2
                               X
    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utl_rbegin;
    parmcards4;
    library(spatstat)
    library(haven)
    source("c:/temp/fn_tosas9.R")
    have<-read_sas("d:/sd1/have.sas7bdat");
    pts <- ppp(have$X, have$Y, c(0, 2), c(0, 2))
    want <- quadratcount(pts, nx=2, ny=2)
    want
    fn_tosas9(dataf=want);
    ;;;;
    %utl_rend;

    libname tmp "c:/temp";
    proc print data=tmp.want(drop=rownames);
    run;quit;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*       x                                                                                                                */
    /*  y     [0,1) [1,2]                                                                                                     */
    /*  [1,2]     2     6                                                                                                     */
    /*  [0,1)     3     7                                                                                                     */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  SAS                                                                                                                   */
    /*                                                                                                                        */
    /*  V_0_1_    V_1_2_                                                                                                      */
    /*                                                                                                                        */
    /*     2         6                                                                                                        */
    /*     3         7                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __ ___ _ __   ___  ___
    | `__/ _ \ `_ \ / _ \/ __|
    | | |  __/ |_) | (_) \__ \
    |_|  \___| .__/ \___/|___/
             |_|
    */

    REPO
    -------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl_digitize_data_from_image
    https://github.com/rogerjdeangelis/utl-R-kmeans-clustering--of-tweets
    https://github.com/rogerjdeangelis/utl-kmeans-cluster-analysis-based-on-only-one-variable-in-r
    https://github.com/rogerjdeangelis/utl-r-kmeans-and-sas-fastclus-to-bucket-continuous-variables
    https://github.com/rogerjdeangelis/utl-R-kmeans-clustering--of-tweets
    https://github.com/rogerjdeangelis/utl-draw-polygons-around-clusters-of-points-r-convex-hulls
    https://github.com/rogerjdeangelis/utl-kmeans-cluster-analysis-based-on-only-one-variable-in-r
    https://github.com/rogerjdeangelis/utl-optimum-number-of-clusters-elbow-plot
    https://github.com/rogerjdeangelis/utl-simple-informative-cluster-analysis-using-sas-interface-to-R
    https://github.com/rogerjdeangelis/utl_grouping_monthly_checking_account_into_two_clusters_by_year
    https://github.com/rogerjdeangelis/utl_linked_and_unlinked_clusters_of_servers_that_have_one_or_more_linkages_in_common

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
R counting the number of points in each cell of a grid
