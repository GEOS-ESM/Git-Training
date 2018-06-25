Recommended environment preparations
   
1. setenv GIT_EDITOR='emacs -nw'
2. ...
   
   
   # CVS initial checkout                       # Git equivalent
    
    setenv CVSROOT ...                          git clone git@github.com:GoddardESM/GEOSsystem.git  GEOSagcm
    cvs checkout GEOSagcm                       cd GEOSagcm
                                                src/set_configuration.sh GEOSagcm
                                                git checkout GEOSagcm
                                                
                                                # or 
                                                
                                                ggit GEOSagcm


    # CVS add new file                          # Git equivalent
    cvs add [-m message] files                  git add files


    # CVS 
                                                
                                                
                                                
    
                                                
