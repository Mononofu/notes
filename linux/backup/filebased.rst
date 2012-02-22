to sync two paths and keep a history, use this script

.. sourcecode:: bash

		#!/bin/bash
		unset PATH
		 
		# USER VARIABLES
		BACKUPDIR=/media/games/backup/mononofu                      # Folder on the backup server
		BACKUP_SOURCE=/home/mononofu
		EXCLUDES=/media/games/backup/mononofu/backup_exclude        # File containing exludes
		DAYS=90                                                     # After how many days shall the backups be deleted?
		 
		# PATH VARIABLES
		CP=/bin/cp;
		MK=/bin/mkdir;
		DATE=/bin/date;
		RM=/bin/rm;
		GREP=/bin/grep;
		RSYNC=/usr/bin/rsync;
		TOUCH=/bin/touch;
		FIND=/usr/bin/find;
		 
		##                                                      ##
		##      --       DO NOT EDIT BELOW THIS HERE     --     ##
		##                                                      ##
		 
		# CREATING CURRENT DATE / TIME
		$MK $BACKUPDIR/current
		$MK $BACKUPDIR/old
		NOW=`$DATE '+%Y-%m'-%d_%H:%M`
		MKDIR=$BACKUPDIR/old/$NOW/
		$MK $MKDIR
		 
		# RUN RSYNC INTO CURRENT
		$RSYNC                                                          \
		        -avzp --delete --delete-excluded                        \
		        --exclude-from="$EXCLUDES"                              \
		        $BACKUP_SOURCE                                          \
		        $BACKUPDIR/current ;
		 
		# UPDATE THE MTIME TO REFELCT THE SNAPSHOT TIME
		$TOUCH $BACKUPDIR/current
		 
		# MAKE HARDLINK COPY
		$CP -al $BACKUPDIR/current/* $MKDIR
		 
		# REMOVE OLD BACKUPS
		for file in "$( $FIND $BACKUPDIR/old/ -maxdepth 1 -type d -mtime +$DAYS )"
		do
		  $RM -Rf $file
		done
		 
		exit 0
