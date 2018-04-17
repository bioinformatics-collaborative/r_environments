# Author: Rob Gilmore, Shaurita Hutchins, and Eric Vallender
#
#
# This .Rprofile is used to get a more specific local library
# path (.libPaths()) for even minor versions of R.  The local library
# path is where bioconductor/cran/github packages are stored in each
# users home directory.
#
# Keeping the local libraries seperate for each minor version of R 
# on the MCSR will keep pipelines from breaking because of dependency
# issues as well as others (see below).
#
#
# R - 3.4.3 is compiled with intel
# R - 3.4.4 is compiled with gcc

#Sys.setenv("R_LIBS"=Sys.getenv("R_LIBS_USER"))
#options(defaultPackages=c(getOption("defaultPackages", "BiocInstaller")))

askYesNo <- function(msg) {
    ansr <- readline(prompt=msg)
    ansr <- tolower(ansr)
    acceptable_ansr <- c("yes", "no", "n", "y")
    while(!any(grep(ansr, acceptable_ansr, fixed=TRUE))) {
    	ansr <- readline(prompt=msg)
	ansr <- tolower(ansr)
    }
    
    if (any(grep(ansr, c("yes", "y")))) {
	ret_log = TRUE
    } else {
    	ret_log = FALSE
    }	
    return(ret_log)
}



local( {
# Only execute on interactive session in a local environment
# This prevents infinite looping
    
    if(interactive()) {
	# Quietly load utils
        require(utils, quietly=TRUE)
	# Prompt the user to make sure they want to install Bioconductor
	warning("Prompting for Bioconductor Installation...\n", immediate.=TRUE, call.=FALSE)
	if (askYesNo("Do you want to install Bioconductor?? [Y/N]")){
    	    # Create the proper directories if they don't already exist
	    if (!dir.exists(Sys.getenv("R_LIBS_USER"))) {
	        dir.create(Sys.getenv("R_LIBS_USER"), recursive=TRUE)
	    }    
	    
	    message("\n..................Attempting to Load Biocondcutor...................\n")
	    # Try to load the Bioconductor Installer
	    x <- try(library("BiocInstaller"), silent=TRUE)
	    # If there's an error then Bioconductor hasn't been installed
	    if(class(x) == "try-error") { 
	        # Add the user library to .libPaths so Bioconductor installs properly
	    	.libPaths(Sys.getenv("R_LIBS_USER"))
		
	    	# **********************************************************
	    	# Options for Downloading the biocLite.R script manually
	    	#system2("rm", args=c("~/biocLite.R"))
	    	#system2("wget", args=c("-O", "~/biocLite.R", "http://bioconductor.org/biocLite.R"), wait=TRUE)
	    	# **********************************************************

	    	# Tell the user what's happening
	    	msg <- sprintf("Installing Bioconductor from %s/.Rprofile.\n", getwd())
		message("")
	    	warning(msg, immediate.=TRUE, call.=FALSE)
	    	# Install Bioconductor
	    	source("https://bioconductor.org/biocLite.R")
		message("\n..............Bioconductor was successfully installed...............\n")
	    } else {
	    	message("\n.................Bioconductor is already installed..................\n")
	    }
	    # Add BiocInstaller to the defaultPackages option
	    options(defaultPackages=c(getOption("defaultPackages"), "BiocInstaller"))
	    # Add Bioconductor to the repos option
	    options(repos = c(getOption('repos')["CRAN"], BiocInstaller:::biocinstallRepos()))
        }  
    }
})

# **********************************************************
# Remove the biocLite.R script from manual installation
#system2("rm", args="~/biocLite.R")
# **********************************************************

# Make sure that bioconductors installation repositories are added
#BiocInstaller::biocLite()

