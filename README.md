# utl-using-sql-arrays-substract-the-values-in-row2-row4-from-row1-by-column
Using sql arrays substract the values in row2 row4 from row1 by column
    Using sql arrays substract the values in row2 row4 from row1 by column                                              
                                                                                                                        
    This can be very fast with a smart compiler.                                                                        
                                                                                                                        
           Two solutions                                                                                                
                                                                                                                        
                a. sas sql arrays                                                                                       
                b. R                                                                                                    
                                                                                                                        
    github                                                                                                              
    https://tinyurl.com/y2ao6be5                                                                                        
    https://github.com/rogerjdeangelis/utl-using-sql-arrays-substract-the-values-in-row2-row4-from-row1-by-column       
                                                                                                                        
    macros                                                                                                              
    https://tinyurl.com/y9nfugth                                                                                        
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                          
                                                                                                                        
    SAS Forum                                                                                                           
    https://tinyurl.com/yyvq9fh5                                                                                        
    https://communities.sas.com/t5/New-SAS-User/Subtracting-rows/m-p/692930                                             
                                                                                                                        
    /*                   _                                                                                              
    (_)_ __  _ __  _   _| |_                                                                                            
    | | `_ \| `_ \| | | | __|                                                                                           
    | | | | | |_) | |_| | |_                                                                                            
    |_|_| |_| .__/ \__,_|\__|                                                                                           
            |_|                                                                                                         
    */                                                                                                                  
                                                                                                                        
    data have;                                                                                                          
     input name$ c1 c2 c3 c4 c5 c6;                                                                                     
    cards4;                                                                                                             
    r1 6 6 6 6 6 6                                                                                                      
    r2 1 1 1 1 1 1                                                                                                      
    r3 2 2 2 2 2 2                                                                                                      
    r4 3 3 3 3 3 3                                                                                                      
    ;;;;                                                                                                                
    run;quit;                                                                                                           
                                                                                                                        
                                                                                                                        
    WORK.HAVE total obs=4    | RULES                                                                                    
                             | Consider column 1                                                                        
                             |                                                                                          
     NAME C1 C2 C3 C4 C5 C6  | S1                                                                                       
                             |                                                                                          
     r1    6  6  6  6  6  6  |  0 = 6-(1+2+3)                                                                           
     r2    1  1  1  1  1  1  |                                                                                          
     r3    2  2  2  2  2  2  |                                                                                          
     r4    3  3  3  3  3  3  |                                                                                          
                                                                                                                        
    /*           _               _                                                                                      
      ___  _   _| |_ _ __  _   _| |_                                                                                    
     / _ \| | | | __| `_ \| | | | __|                                                                                   
    | (_) | |_| | |_| |_) | |_| | |_                                                                                    
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                   
                    |_|                                                                                                 
    */                                                                                                                  
                                                                                                                        
    WORK.WANT total obs=1                                                                                               
                                                                                                                        
     S1 S2 S3 S4 S5 S6                                                                                                  
                                                                                                                        
      0  0  0  0  0  0                                                                                                  
                                                                                                                        
    /*         _       _   _                                                                                            
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                 
    / __|/ _ \| | | | | __| |/ _ \| `_ \                                                                                
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                               
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                               
                         _                                                                                              
      __ _     ___  __ _| |   __ _ _ __ _ __ __ _ _   _ ___                                                             
     / _` |   / __|/ _` | |  / _` | `__| `__/ _` | | | / __|                                                            
    | (_| |_  \__ \ (_| | | | (_| | |  | | | (_| | |_| \__ \                                                            
     \__,_(_) |___/\__, |_|  \__,_|_|  |_|  \__,_|\__, |___/                                                            
                      |_|                         |___/                                                                 
    */                                                                                                                  
                                                                                                                        
    * make data;                                                                                                        
                                                                                                                        
    options validvarname=upcase;                                                                                        
    libname sd1 "d:/sd1";                                                                                               
    data sd1.have;                                                                                                      
     input name$ c1 c2 c3 c4 c5 c6;                                                                                     
    cards4;                                                                                                             
    r1 6 6 6 6 6 6                                                                                                      
    r2 1 1 1 1 1 1                                                                                                      
    r3 2 2 2 2 2 2                                                                                                      
    r4 3 3 3 3 3 3                                                                                                      
    ;;;;                                                                                                                
    run;quit;                                                                                                           
                                                                                                                        
    %array(cs,values=1-6);                                                                                              
                                                                                                                        
    proc sql;                                                                                                           
       create                                                                                                           
          table want as                                                                                                 
       select                                                                                                           
         %do_over(cs,phrase=%str(                                                                                       
          (select sum(c?) from have where name eq "r1") -                                                               
          (select sum(c?) from have where name ne "r1") as s?),BETWEEN=COMMA)                                           
       from                                                                                                             
          have(obs=1)                                                                                                   
    ;quit;                                                                                                              
                                                                                                                        
    /*                                                                                                                  
    | |__     _ __                                                                                                      
    | `_ \   | `__|                                                                                                     
    | |_) |  | |                                                                                                        
    |_.__(_) |_|                                                                                                        
                                                                                                                        
    */                                                                                                                  
                                                                                                                        
    %utl_submit_r64('                                                                                                   
    library(haven);                                                                                                     
    library(SASxport);                                                                                                  
    have<-read_sas("d:/sd1/have.sas7bdat")[,-1];                                                                        
    str(have);                                                                                                          
    want<-have[1,] - colSums(have[-1,]);                                                                                
    want;                                                                                                               
    write.xport(want,file="d:/xpt/want.xpt");                                                                           
    ');                                                                                                                 
                                                                                                                        
    %utlfkil(d/xpt/want.xpt);                                                                                           
                                                                                                                        
    proc datasets lib=work nolist;                                                                                      
     delete want;                                                                                                       
    run;quit;                                                                                                           
                                                                                                                        
    libname xpt xport "d:/xpt/want.xpt";                                                                                
    data want;                                                                                                          
      set xpt.want(rename=(c1-c6=s1-s6));                                                                               
    run;quit;                                                                                                           
    libname xpt clear;                                                                                                  
                                                                                                                        
