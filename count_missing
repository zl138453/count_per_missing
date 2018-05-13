
%macro count_per_missing(ds);
proc delete data=result1;
run;

proc contents
     data = &ds.
          noprint
          out = aa
               (keep = name varnum);
run;

proc sort data=aa;
by VARNUM;
run;

proc sql;
create table number as
select max(VARNUM) as n
from aa;
quit;

data _null_;
set number;
call symput('n',n);
run;


%do i = 1 %to &n.;

data _null_;
set aa;
if VARNUM=&i.;
call symput('c',NAME);
run;

proc freq data=&ds.;
tables &c./out=cc;
run;


proc sql;
create table bb as
select COUNT/m as missing_ratio 
from (select sum(COUNT) as m from cc c)a, cc b where b.&c. is null;
quit;

data dd;
set bb;
length name $20.;
name="&c.";
run;

proc append data=dd base=result1 force;
run;

%end;
proc sql;
create table result as
select a.*, b.missing_ratio
from aa a left join result1 b on a.name=b.name;
quit;

data result;
set result;
if missing_ratio=. then missing_ratio=1;
run;
proc sort data=result;
by VARNUM;
run;

%mend;

