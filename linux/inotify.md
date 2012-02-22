Perform action upon creation of file
====================================

		inotifywait -qme CREATE --exclude "crdownload" --format '%w%f' /home/mononofu/Downloads | grep --line-buffered .nzb | while read f; do mv "$f" /media/nas/downloads/queue; done
		
Important: grep needs ”–line-buffered” or it will wait till buffer is full to print to pipe!