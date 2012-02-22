
[fsarchiver](http://www.fsarchiver.org/Main_Page) creates images of whole partitions, can read ext4 and ntfs, is on system rescue cd

to make a backup of sda1:

    fsarchiver savefs /path/to/backup/file.fsa /dev/sda1 -j4

and restore it again:

    fsarchiver restfs /path/to/backup/file.fsa id=0,dest=/dev/sda1