# utl-get-dataset-attributes-at-macro-time-within-a-datastep-using-attrn-dosubl-macros
Getting dataset atttributes at macro time using dosubl and Roland Rashleigh_Berry(RIP) macros 
    %let pgm=utl-get-dataset-attributes-at-macro-time-within-a-datastep-using-attrn-dosubl-macros;

    %stop_submission;

    Getting dataset atttributes at macro time using dosubl and Roland Rashleigh_Berry(RIP) macros

    github
    http://tinyurl.com/5ent9jbu
    https://github.com/rogerjdeangelis/utl-get-dataset-attributes-at-macro-time-within-a-datastep-using-attrn-dosubl-macros

    These functions can be embedded in a datastep and are able to generate code
    Three macros to get meta data on wps/sas datasets within a datastep.

     EXAMPLES (all at macro time)

     1 ATTRN    If the config was updated today then apply the transactions to the master
     2 ATTRC    Within a dtatstep get the libname associated wit the input dataset
     3 DATASTEP Indirect Addressing
     4 DOSUBL   Extracting the number of variables and observations in the input datastep within a datastep
                Create a subscripted array vs[&size]. Note I realize there are simpler solutions (of _numeric_),
                But there are mnay problems where the size of the array is not available within the datastep.
     5 ATTRC ATTRN DOSUBL MACROS on end of this message


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* NOTE ATTRN/C NAMES ARE NOT THE SAME AS VARIABLE NAMES IN THE DICTIONARIES, IE CRDTE VS CRDATE RESPECTIVELY             */
    /*===========================================================================================================             */
    /*        _   _                           _   _                    __                           _                         */
    /*   __ _| |_| |_ _ __ _ __          __ _| |_| |_ _ __ ___        / _|_   _ _ __   ___   __   _(_) _____      _____       */
    /*  / _` | __| __| `__| `_ \        / _` | __| __| `__/ __|      | |_| | | | `_ \ / __|  \ \ / / |/ _ \ \ /\ / / __|      */
    /* | (_| | |_| |_| |  | | | |      | (_| | |_| |_| | | (__       |  _| |_| | | | | (__    \ V /| |  __/\ V  V /\__ \      */
    /*  \__,_|\__|\__|_|  |_| |_|       \__,_|\__|\__|_|  \___|      |_|  \__,_|_| |_|\___|    \_/ |_|\___| \_/\_/ |___/      */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* NUMERIC ATTRIBUTES FOR ATTRN   CHARACTER ATTRIBUTES FOR ATTRC   DATASTEP FUNCTIONS      DICTIONARY VIEWS               */
    /* ============================   ========================== ====   ==================      ================              */
    /*                                                                                                                        */
    /* see for details                 see for details                                                                        */
    /* http://tinyurl.com/2d7n6wxu     http://tinyurl.com/2r9c5j9z                                                            */
    /*                                                                                                                        */
    /* Usefull                         Usefull                          Usefull                 Usefull                       */
    /*                                                                                                                        */
    /* CRDTE                           LABEL                            VNAMEX                  TABLE                         */
    /* MODTE                           LIB                              VLABELX                 VCOLUMN                       */
    /* NOBS                            MEM                              VVALUEX                                               */
    /* NVARS                           SORTEDBY                         VTYPEX                                                */
    /* NLOBS                           ENGINE                           VLENGTHX                                              */
    /* INDEX                                                                                                                  */
    /*                                                                  VAR                     VFORMAT                       */
    /* Less Useful                     Less Useful                      VARNAME                 VEXTFL                        */
    /*                                                                  VLENGTH                 VMACRO                        */
    /* ALTERPW                         CHARSET                          VARLABEL                VGOPT                         */
    /* ANOBS                           COMPRESS                         VTYPE                   VTITLE                        */
    /* ANY                             DATAREP                          VVALUE                  VOPTION                       */
    /* ARAND                           ENCODING                         VARTYPE                                               */
    /* ARWU                            ENCRYPT                          VARFMT                                                */
    /* AUDIT                           MODE                                                     VLIBNAM                       */
    /* AUDIT_DATA                      MTYPE                            VARINFMT                VCATALG                       */
    /* AUDIT_BEFOR                     SORTLVL                          VARLEN                  VCFORMAT                      */
    /* AUDIT_ERROR                     SORTSEQ                          VARNUM                  VDCTNRY                       */
    /* ICONST                                                           VARRAY                  VDEST                         */
    /* ISINDEX                                                          VARRAYX                 VINDEX                        */
    /* ISSUBSET                                                         VERIFY                  VMEMBER                       */
    /* LRECL                                                            VFORMAT                 VVIEW                         */
    /* LRID                                                             VFORMATD                                              */
    /* MAXGEN                                                           VFORMATDX                                             */
    /* MAXRC                                                            VFORMATN                VSACCES                       */
    /* NDEL                                                             VFORMATNX               VSCATLG                       */
    /* NEXTGEN                                                          VFORMATW                VSELIB                        */
    /* NLOBSF                                                           VFORMATWX               VSEMEM                        */
    /* PW                                                               VFORMATX                VSLIB                         */
    /* RADIX                                                            VINARRAY                VSTABLE                       */
    /* READPW                                                           VINARRAYX               VSTABVW                       */
    /* REUSE                                                            VINFORMAT               VSVIEW                        */
    /* TAPE                                                             VINFORMATD                                            */
    /* WHSTMT                                                           VINFORMATDX                                           */
    /*                                                                  VINFORMATN                                            */
    /*                                                                  VINFORMATNX                                           */
    /*                                                                  VINFORMATW                                            */
    /*                                                                  VINFORMATWX                                           */
    /*                                                                  VINFORMATX                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*        _                                                                                                               */
    /* __   _(_) _____      _____                                                                                             */
    /* \ \ / / |/ _ \ \ /\ / / __|                                                                                            */
    /*  \ V /| |  __/\ V  V /\__ \                                                                                            */
    /*   \_/ |_|\___| \_/\_/ |___/                                                                                            */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* LIBNAME    MEMNAME     MEMTYPE  NVAR SUBSET OF USEFULL VARIABLES                                                       */
    /*                                                                                                                        */
    /* SASHELP    VTABLE       VIEW     27  libname memname memtype memlabel typemem crdate modate nobs obslen                */
    /*                                      nvar compress npage filesize pcompress delobs nlobs maxvar maxlabel               */
    /*                                      indxtype datarep sortname sorttype datarepname encoding num_character             */
    /*                                      num_numeric pointobs                                                              */
    /*                                                                                                                        */
    /* SASHELP    VCOLUMN      VIEW     18  libname memname memtype name type length npos varnum label                        */
    /* SASHELP    VFORMAT      VIEW     13  libname memname path objname fmtname fmttype source minw mind maxw maxd defw defd */
    /* SASHELP    VEXTFL       VIEW      3  fileref xpath xengine                                                             */
    /* SASHELP    VMACRO       VIEW      5  scope name offset value readonly                                                  */
    /* SASHELP    VGOPT        VIEW      6  optname opttype setting optdesc level group                                       */
    /* SASHELP    VTITLE       VIEW      3  type number text                                                                  */
    /* SASHELP    VOPTION      VIEW      6  optname opttype setting optdesc level group                                       */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* SASHELP    VLIBNAM      VIEW     10  libname engine path  readonly sequential                                          */
    /* SASHELP    VCATALG      VIEW      9  libname memname memtype objname objtype objdesc                                   */
    /* SASHELP    VCFORMAT     VIEW     13  libname memname path objname fmtname fmttype source                               */
    /* SASHELP    VDCTNRY      VIEW     10  memname memlabel name type length npos varnum label format informat               */
    /* SASHELP    VDEST        VIEW      2  destination style                                                                 */
    /* SASHELP    VINDEX       VIEW      9  libname memname memtype name idxusage indxname indxpos nomiss unique              */
    /* SASHELP    VMEMBER      VIEW      6  libname memname memtype engine index path                                         */
    /* SASHELP    VVIEW        VIEW      4  libname memname memtype engine                                                    */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* SASHELP    VSACCES      VIEW      2  libname memname                                                                   */
    /* SASHELP    VSCATLG      VIEW      2  libname memname                                                                   */
    /* SASHELP    VSELIB       VIEW      4  libname engine level path                                                         */
    /* SASHELP    VSEMEM       VIEW      4  libname memname memtype path                                                      */
    /* SASHELP    VSLIB        VIEW      2  libname path                                                                      */
    /* SASHELP    VSTABLE      VIEW      2  libname memname                                                                   */
    /* SASHELP    VSTABVW      VIEW      3  libname memname memtype                                                           */
    /* SASHELP    VSVIEW       VIEW      2  libname memname                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

     /*                  _
    (_)_ __  _ __  _   _| |_ ___
    | | `_ \| `_ \| | | | __/ __|
    | | | | | |_) | |_| | |_\__ \
    |_|_| |_| .__/ \__,_|\__|___/
            |_|       _  _         _        _     _
      ___ ___  _ __  / _(_) __ _  | |_ __ _| |__ | | ___
     / __/ _ \| `_ \| |_| |/ _` | | __/ _` | `_ \| |/ _ \
    | (_| (_) | | | |  _| | (_| | | || (_| | |_) | |  __/
     \___\___/|_| |_|_| |_|\__, |  \__\__,_|_.__/|_|\___|
                           |___/
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";

    /*----                                                                   ----*/
    /*---- modified on 21JAN2024                                             ----*/
    /*----                                                                   ----*/

    data sd1.config;
      informat trial $3. rundate date9. status $10.;
      input trial rundate status;
    cards4;
    ABC  19JAN2024  COMPLETED
    ABC  20JAN2024  EXECUTED
    ;;;;
    run;quit;

    /*                                  _
    | |_ _ __ __ _ _ __  ___  __ _  ___| |_
    | __| `__/ _` | `_ \/ __|/ _` |/ __| __|
    | |_| | | (_| | | | \__ \ (_| | (__| |_
     \__|_|  \__,_|_| |_|___/\__,_|\___|\__|

    */

    data sd1.transact;
      set sashelp.class(obs=5);
    run;quit;

    /*                              _
      _____  ____ _ _ __ ___  _ __ | | ___  ___
     / _ \ \/ / _` | `_ ` _ \| `_ \| |/ _ \/ __|
    |  __/>  < (_| | | | | | | |_) | |  __/\__ \
     \___/_/\_\__,_|_| |_| |_| .__/|_|\___||___/
                             |_|
    */

    /*         _   _
    / |   __ _| |_| |_ _ __ _ __
    | |  / _` | __| __| `__| `_ \
    | | | (_| | |_| |_| |  | | | |
    |_|  \__,_|\__|\__|_|  |_| |_|

    */

    /*----                                                                   ----*/
    /*---- update master if config modification date = todays date           ----*/
    /*----                                                                   ----*/

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete update_master; run;quit;

    %utl_submit_wps64x('
    options sasautos=("c:/oto",sasautos);
    libname sd1 "d:/sd1";
    data sd1.update_master;

      if today() = datepart(%attrn(sd1.config,modte)) then set sd1.transact;
      else putlog "master not updated";

    run;quit;
    proc print data=sd1.update_master;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.UPDATE_MASTER                                                                                                      */
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs     NAME      SEX    AGE    HEIGHT    WEIGHT                                                                       */
    /*                                                                                                                        */
    /*  1     Alfred      M      14     69.0      112.5                                                                       */
    /*  2     Alice       F      13     56.5       84.0                                                                       */
    /*  3     Barbara     F      13     65.3       98.0                                                                       */
    /*  4     Carol       F      14     62.8      102.5                                                                       */
    /*  5     Henry       M      14     63.5      102.5                                                                       */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___          _   _
    |___ \    __ _| |_| |_ _ __ ___
      __) |  / _` | __| __| `__/ __|
     / __/  | (_| | |_| |_| | | (__
    |_____|  \__,_|\__|\__|_|  \___|

    */

    /*----                                                                   ----*/
    /*---- add libname of associated withrogram                              ----*/
    /*----                                                                   ----*/

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    options sasautos=("c:/oto",sasautos);
    libname sd1 "d:/sd1";
    data sd1.want;

       retain input_libname ;

       set sd1.transact;

       input_libname = "%attrc(sd1.transact,lib)";

    run;quit;
    proc print data=sd1.update_master;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*         ADDED                                                                                                          */
    /*         VARIABLE                                                                                                       */
    /*  Obs    INPUT_LIBNAME     NAME      SEX    AGE    HEIGHT    WEIGHT                                                     */
    /*         =============                                                                                                  */
    /*   1          SD1         Alfred      M      14     69.0      112.5                                                     */
    /*   2          SD1         Alice       F      13     56.5       84.0                                                     */
    /*   3          SD1         Barbara     F      13     65.3       98.0                                                     */
    /*   4          SD1         Carol       F      14     62.8      102.5                                                     */
    /*   5          SD1         Henry       M      14     63.5      102.5                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____    __                                      _
    |___ /   / _|_   _ _ __   ___  __   ____   ____ _| |_   _  ___
      |_ \  | |_| | | | `_ \ / __| \ \ / /\ \ / / _` | | | | |/ _ \
     ___) | |  _| |_| | | | | (__   \ V /  \ V / (_| | | |_| |  __/
    |____/  |_|  \__,_|_| |_|\___|   \_/    \_/ \__,_|_|\__,_|\___|

    */
    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    /*----                                                                   ----*/
    /*---- INPUT                                                             ----*/
    /*----                                                                   ----*/

    options validvarname=upcase;
    libname sd1 "d:/sd1";

    data sd1.have;
     input VARA$ V1 V2 V3 V4;
    cards4;
    V1 2 4 6 8
    V3 2 4 6 8
    V4 2 4 6 8
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs    VAX    V1    V2    V3    V4    V5                                                                              */
    /*                                                                                                                        */
    /*   1     V2      1     2     3     4     5                                                                              */
    /*   2     V4      1     2     3     4     5                                                                              */
    /*   3     V5      1     2     3     4     5                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*----                                                                   ----*/
    /*----  USING INDIRECT ADDRESSING                                        ----*/
    /*----                                                                   ----*/

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;
    proc datasets lib=work nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    data sd1.want;

        length vara vx $8;
        set sd1.have;
        vx  =  left(vvaluex(vara));

    run;
    proc print;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.WANT total obs=3                                                                                                   */
    /*                                                                                                                        */
    /*  VARA    VX    V1    V2    V3    V4   VX Indirect Adddressing (VX is a character type)                                 */
    /*                                                                                                                        */
    /*   V1     2      2     4     6     8   VARA -> V1 -> 2                                                                  */
    /*   V3     6      2     4     6     8   VARA -> V2 -> 6                                                                  */
    /*   V4     8      2     4     6     8   VARA -> V2 -> 8                                                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    libname sd1 'd:/sd1';
    data want;
      retain cum rowtot 0;
      if _n_=0 then do;

         %dosubl('
           select
               num_numeric
           into
              :_num_numeric
           from
               sashelp.vtable
           where
             memname     = "TRANSACT"
             and libname = "SD1"
           ;quit;
           ');
       end;

       set sd1.transact;

       /*----                                                                ----*/
       /*---- The size of the ns array requires the macro time dosubl        ----*/
       /*----                                                                ----*/

       array ns[&_num_numeric] %utl_varlist(sd1.transact,keep=_numeric_);

       do i = 1 to dim(ns);
         rowtot = rowtot + ns[i];
       end;

       cum = cum + rowtot;

       drop i;

    run;quit;

    proc print;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  CUM and ROWTOT COMPUTED                                                                                               */
    /*                                                                                                                        */
    /*  WORK.WANT total obs=5                                                                                                 */
    /*                                                                                                                        */
    /*  Obs      CUM     ROWTOT     NAME      SEX    AGE    HEIGHT    WEIGHT    I                                             */
    /*                                                                                                                        */
    /*   1      195.5     195.5    Alfred      M      14     69.0      112.5    4                                             */
    /*   2      544.5     349.0    Alice       F      13     56.5       84.0    4                                             */
    /*   3     1069.8     525.3    Barbara     F      13     65.3       98.0    4                                             */
    /*   4     1774.4     704.6    Carol       F      14     62.8      102.5    4                                             */
    /*   5     2659.0     884.6    Henry       M      14     63.5      102.5    4                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*     _   _
      __ _| |_| |_ _ __ ___
     / _` | __| __| `__/ __|
    | (_| | |_| |_| | | (__
     \__,_|\__|\__|_|  \___|

    */

    filename ft15f001 "c:/otowps/attrc.sas";
    parmcards4;
    /*<pre><b>
    / Program   : attrc
    / Version   : 1.0
    / Author    : Roland Rashleigh-Berry
    / Date      : October 2002
    / Contact   : rolandberry@hotmail.com
    / SubMacros : none
    / Purpose   : To return a character attribute of a dataset
    / Notes     : This is a low-level utility macro that other shell macros will call
    /             About all you would use this for is to get the dataset label and
    /             the variables a dataset is sorted by.
    / Usage     : %let dslabel=%attrc(dsname,label);
    /             %let sortseq=%attrc(dsname,sortedby);
    /
    /================================================================================
    / PARAMETERS:
    /-------name------- -------------------------description-------------------------
    / ds                Dataset name (pos)
    / attrib            Attribute (pos)
    /================================================================================
    / AMENDMENT HISTORY:
    / init --date-- mod-id ----------------------description-------------------------
    /
    /================================================================================
    / This is public domain software. No guarantee as to suitability or accuracy is
    / given or implied. User uses this code entirely at their own risk.
    /===============================================================================*/

    %macro attrc(ds,attrib);

    %local dsid rc;

    %let dsid=%sysfunc(open(&ds,is));
    %if &dsid EQ 0 %then %do;
      %put ERROR: (attrc) Dataset &ds not opened due to the following reason:;
      %put %sysfunc(sysmsg());
    %end;
    %else %do;
    %sysfunc(attrc(&dsid,&attrib))
      %let rc=%sysfunc(close(&dsid));
    %end;

    %mend;
    ;;;;
    run;quit;



    /*     _   _
      __ _| |_| |_ _ __ _ __
     / _` | __| __| `__| `_ \
    | (_| | |_| |_| |  | | | |
     \__,_|\__|\__|_|  |_| |_|

    */

    filename ft15f001 "c:/otowps/attrn.sas";
    parmcards4;
    / Program   : attrn
    / Version   : 1.0
    / Author    : Roland Rashleigh-Berry
    / Date      : October 2002
    / Contact   : rolandberry@hotmail.com
    / Purpose   : To return a numeric attribute of a dataset
    / SubMacros : none
    / Notes     : This is a low-level utility macro that other shell macros will call
    /             The full list of attributes can be found in the SAS documentation.
    /             The most common ones used will be CRDTE and MODTE (creation and
    /             last modification date), NOBS and NLOBS (number of observations and
    /             number of logical [i.e. not marked for deletion] observations) and
    /             NVARS (number of variables).
    / Usage     : %let nobs=%attrn(dsname,nlobs);
    /
    /================================================================================
    / PARAMETERS:
    /-------name------- -------------------------description-------------------------
    / ds                Dataset name (pos)
    / attrib            Attribute (pos)
    /================================================================================
    / AMENDMENT HISTORY:
    / init --date-- mod-id ----------------------description-------------------------
    /
    /================================================================================
    / This is public domain software. No guarantee as to suitability or accuracy is
    / given or implied. User uses this code entirely at their own risk.
    /===============================================================================*/

    %macro attrn(ds,attrib);

    %local dsid rc;

    %let dsid=%sysfunc(open(&ds,is));
    %if &dsid EQ 0 %then %do;
      %put ERROR: (attrn) Dataset &ds not opened due to the following reason:;
      %put %sysfunc(sysmsg());
    %end;
    %else %do;

    %left(%sysfunc(attrn(&dsid,&attrib)))
      %let rc=%sysfunc(close(&dsid));
    %end;

    %mend attrn;
    ;;;;
    run;quit;

    /*   _                 _     _
      __| | ___  ___ _   _| |__ | |
     / _` |/ _ \/ __| | | | `_ \| |
    | (_| | (_) \__ \ |_| | |_) | |
     \__,_|\___/|___/\__,_|_.__/|_|

    */

    filename ft15f001 "c:/otowps/dosubl.sas";
    parmcards4;
    %macro dosubl(arg);
      %let rc=%qsysfunc(dosubl(&arg));
    %mend dosubl;
    ;;;;
    run;quit;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
