# utl-Keep-group-of-records-if-first-value-is-zero
Keep group of records if first value is zero
    SAS Forum: Keep group of records if first value is zero                                                                    
                                                                                                                               
    First and last processing is a SAS differentiator.                                                                         
    This solution demonstrates how elegant and simple                                                                          
    the processing can be                                                                                                      
                                                                                                                               
    Solution by                                                                                                                
    Paul Dorfman                                                                                                               
    sashole@bellsouth.net                                                                                                      
                                                                                                                               
    github                                                                                                                     
    https://tinyurl.com/y4uxnmes                                                                                               
    https://github.com/rogerjdeangelis/utl-Keep-group-of-records-if-first-value-is-zero                                        
                                                                                                                               
    SAS Forum                                                                                                                  
    https://tinyurl.com/y2oq3goq                                                                                               
    https://communities.sas.com/t5/SAS-Programming/Dropping-all-observations-of-an-ID-if-first-meets-condition/m-p/580984      
                                                                                                                               
    I have mostly non-SAS programmers following my github so                                                                   
    I like to add simple elegant SAS differetiators.                                                                           
                                                                                                                               
    *_                   _                                                                                                     
    (_)_ __  _ __  _   _| |_                                                                                                   
    | | '_ \| '_ \| | | | __|                                                                                                  
    | | | | | |_) | |_| | |_                                                                                                   
    |_|_| |_| .__/ \__,_|\__|                                                                                                  
            |_|                                                                                                                
    ;                                                                                                                          
                                                                                                                               
    data have ;                                                                                                                
      input PID Gender Income Year Exercise ;                                                                                  
      cards ;                                                                                                                  
    1  1    300  2000   0                                                                                                      
    1  1    400  2002   1                                                                                                      
    1  1  50001  2004   2                                                                                                      
    2  2  90000  2000   4                                                                                                      
    2  2   5999  2002  10                                                                                                      
    2  2   3443  2004   0                                                                                                      
    3  2   1333  2000   2                                                                                                      
    3  2   4000  2002   3                                                                                                      
    3  2   5000  2004  10                                                                                                      
    run ;                                                                                                                      
                                                                                                                               
    WORK.HAVE total obs=9                                                                                                      
                                                                                                                               
     PID    GENDER    INCOME    YEAR    EXERCISE  | RULES                                                                      
                                                  |                                                                            
      1        1         300    2000        0     | Keep this group                                                            
      1        1         400    2002        1     | because first exercise=0                                                   
      1        1       50001    2004        2     |                                                                            
                                                  |                                                                            
      2        2       90000    2000        4     | Delete group                                                               
      2        2        5999    2002       10     | first exercise=4                                                           
      2        2        3443    2004        0     |                                                                            
                                                  |                                                                            
      3        2        1333    2000        2     | Delete group                                                               
      3        2        4000    2002        3     | first exercise=1                                                           
      3        2        5000    2004       10     |                                                                            
                                                  |                                                                            
    *            _               _                |                                                                            
      ___  _   _| |_ _ __  _   _| |_              |                                                                            
     / _ \| | | | __| '_ \| | | | __|                                                                                          
    | (_) | |_| | |_| |_) | |_| | |_                                                                                           
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                          
                    |_|                                                                                                        
    ;                                                                                                                          
                                                                                                                               
    WORK.WANT total obs=3                                                                                                      
                                                                                                                               
     PID    GENDER    INCOME    YEAR    EXERCISE                                                                               
                                                                                                                               
      1        1         300    2000        0                                                                                  
      1        1         400    2002        1                                                                                  
      1        1       50001    2004        2                                                                                  
                                                                                                                               
                                                                                                                               
    *          _       _   _                                                                                                   
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                        
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                       
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                      
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                      
                                                                                                                               
    ;                                                                                                                          
                                                                                                                               
    * nice;                                                                                                                    
    * _iorc_ is retained and is set to 1 in exercise=0;                                                                        
                                                                                                                               
    data want ;                                                                                                                
      set have ;                                                                                                               
      by pid ;                                                                                                                 
      if first.pid then _iorc_ = ifn (exercise, 0, 1) ;                                                                        
      if _iorc_ ;                                                                                                              
    run ;                                                                                                                      
                                                                                                                               
                                                                                                                               
                                                                                                                               
                                                                                                                               
