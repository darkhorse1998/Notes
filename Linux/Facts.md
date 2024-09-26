# Facts

1. In Linux, the systemctl daemon-reload command reloads the systemd configuration files, including unit files and security profiles, without restarting the system or services.
2. Systemd is a system and service manager for Linux.
3. Change directory: \
`cd /` -> Takes you to root directory \
`cd -` -> Takes you to previous directory \
`cd` -> Takes you to home directory
4. Inode stores important details abou the files in the file system. The data of a file might be scattered all around the memory. Inode stores all these details and help access it through **links**
5. **Hard links** are links of paths to inodes. \
For example, if a file named _doc.html_ stored at _/home/user1/files/_ directory for user1 and user2 needs the access to it at path _/home/user2/files/_, instead of copying it one can simply create a hard link to it. This will save a lot of memory. \
`ln /home/user1/files/doc.html /home/user2/files/doc.html` creates a hard link to the ionode of the file doc.html. We can verify it by running `stat /home/user1/files/doc.html` \
Now if user1 deletes the file doc.html at his end, only the hard link will be deleted but file will be still accessible to user2. But, if user2 deletes it to, and if there are no hard links attached to the inode of the file, it will be erased from the disk (0 links).
6. Hard links can be used only for files and not for folders. Hard links can be used on the same file system. Proper file permissions should be there to create hard links.
7. Soft links are just shortcuts. You can create soft links by `ln -s <path-to-target-file> <path-to-link-file-or-shortcut>` \
Example: `ln -s /home/user1/files/doc.html doc.html` 
8. In case of softlink, the permissions of the destination file matters.
9. Soft links can be used to point to folders along with files. They can also be used to access files on a different file system, like a mounted file system.
10. `cp -a` preserves everything about the files while copying, like links etc.
11. `stat <file-or-folder>` gives a lot of details about them.
12. Use 'groups' command to see which groups the current user belongs to. Use `chgroup <group> <file>` to change the group associatd with the file.
13. Only the root user can change the user owner of a file/folder. Use `sudo chown <user owner> <file>`
14. Use `sudo chown <user>:<group> <file>` to change the user owner and group togther.
15. Permission are in the form of 3 letters, <user><group><others>. r (read), w (write), x (execute), -(no permission). Permissions are evaluated in a linear fashion from left to right (u -> g -> o). \
Example: Say, file.txt has permissions of _r--rw----_. Who is trying to access the file -> Sam. Who is same -> Owner of the file. Can Sam read the file -> Yes. Can Sam write on the file -> No, as Sam as user owner do not have write permission.
16. We need execute permission in a directory where we want to `cd` into.
17. To change permission of a file/folder we use `chmod` command, like `chmod <permisions> <>file` \
Examples: \
Addition, removal: `chmod u+w file1`, `chmod u-w file1`, `chmod u+rwx file1`, `chmod g-rwx file` \
Exact permissions: `chmod u=w file1`, `chmod g=rw file1`, `chmod g= file1`
Combination: `chmod u+rx,g=w,o= file`
Octal values: `chmod 640 file` \
r (4), w(2), x(1) or other way of looking into it is by set and unset bits. If the permission for user is rx- -> r is set, w is set, x is not set -> Binary 110 -> Decimal value 6
18. When SUID is set for a file, whenever the file is executed, it will be executed as the owner (user) of the file, irrespective of who executes it. SUID digit is 4. If execute permission is already there then it will apear as a lowercase 's', else an uppercase 'S'. \
Example: `chmod 4764 file`
19. When SGID is set for a file, whenever the file is executed, it will be executed as the group, irrespective of who executes it. SGID digit is 2. If execute permission is already there then it will apear as a lowercase 's', else an uppercase 'S'. \
Example: `chmod 2765 file`
20. To see files with SUID or SGID set, run `find <path> -perm /<4/2>000` \
Example: `find . -perm /4000` or `find . -perm /2000`
21. You can set both SUID (4) and SGID (4) on a file by `chmod 6664 file`.
22. To find files set with SUID or SGID or both, run `find <path> -perm /6000`
23. Sticky bit is used to ensure that files inside a directory can be removed only by the user (owner) no mater what permissions have been set. Sticky bit digit is 1. \
Example: `chmod 1777 myDir` or `chmod +t myDir`
24. `find` can be used to find files with certain serach parameters. If not directory is specified it will look in the current directory. \
Examples: \
`find /usr/share -name file.jpg` \
`find -iname file` -> ignores case \
`find -name "i*"` -> searches files starting with 'i' \
`find -mmin 5` -> will list files modified at the 5th minute before the current time \
`find -mmin -5` -> will list all files modified within the last 5 minutes \
`find -mmin +5` -> will list all files modified in the infinite past till the last 5 minutes \
`find -mtime 0` -> will list files modified in the last 24 hours \
`find -mtime 1` -> will list files modified between last 24 to 48 hours \
`find -ctime 0` -> will list files where meta data like file permissions have been modified in the last 24 hours \
`find -size 512k` -> will list files with size 512 KB (c: bytes, k: kilobytes, M: megabytes, G: gigabytes) \
`find -size +512k` -> will list files with size greater than 512 KB \
`find -size -512k` -> will list files with size lesser than 512 KB \
`find -name "f*" -size +512k` -> AND logic \
`find -name "f*" -o -size +512k` -> OR logic \
`find -not -name "f*"` or `find \! -name "f*"` -> list all files whose name do not begin with 'f' \
`find -perm 664` or `find -perm u=rw,g=rw,o=r` -> list files with mentioned permissions \
`find -perm -664` or `find -perm -u=rw,g=rw,o=r` -> list files which have atleast these permissions \
`find -perm /664` or `find -perm /u=rw,g=rw,o=r` -> list files which have any of these permissions \
`find -perm -u=rw,g=rw,o=r` \
`find -type f` -> finds files \
`find -type d` -> finds directories
25. `tac` command is used to print out a file in reverse order (bottom to top)
26. `sed` is used to modify contents in a file. It is a stream editor. Query should b wrapped in quotes. In case / cannot be used, use ~\
Examples: \
`sed 's/canda/canada/gi' file.txt` -> s means substitute (search & replace). g is used to do the substirution on all occurances. This would be just a preview and not actually make the changes. i would make it ignore case \
`sed -i 's/canda/canada/g' file.txt` -> -i (--in-place) ensures the changes are actually made to the file. \
27. `cut` is used to extract parts of the file. \
Examples: \
`cut -d ' ' -f 1 file.txt` -> extracts the 1st element in the elements separated by space in all the lines
27. `uniq file.txt` helps to remove **adjacent** duplicate entries from the file. To make it remove all duplicate elements, first sort the file using `sort file.txt` and then run the `uniq file.txt` command or just do it like this `sort file.txt | uniq`
28. `diff file1 file2` to get the differences. `diff -c file1 file2` gives contex of the lines around the result lines. `diff -y file1 file2` or `sdiff file1 file2` is used to compare the files side by side and differences are marked by '|'
29. Pagers can help in viewing an navigation of files. `less` and `more` are such examples. "/" can be used to search and --i to ignore case. Press enter for next and shift+enter for previous.
30. `grep -irvwo 'lib' /etc/` -> will ignore case (i), look recurssively (r), inverse the search or not command (v), match only words lib and not library (w, --words), output only lib and no context lines (o, --only-matching) \
Examples: \
`grep '^sam' file.txt` -> finds entries that start with 'sam' \
`grep 'sam$' file.txt` -> finds entries that end with 'sam' \
`grep 'c.t' /home/bob/` -> dot (.) would match with any character like cat, cut \
`grep 'c*t' /home/bob/` -> * will match to 0 or any number of characters like caught, colt \
`grep '0\+' /etc` -> 0 should be there at least once
31. Extended grep can help avoid the escape characters we use like \+, \. etc. \
Examples:
`grep -Er '0+' /etc/` or `egrep -r '0+' /etc/` \
`egrep -r '0{3}' /etc/` -> 3 zeros
`egrep -r '0{3,}' /etc/` -> at least 3 zeros \
`egrep -r '0{,3}' /etc/` -> at most 3 zeroes \
`egrep -r '0{1,3}' /etc/` -> min 1, max 3 zeroes \
`egrep -r 'cat?' /etc/` -> ? makes last character optional \
`egrep -r 'cat|dog' /etc/` -> Or logic \
`egrep -r 'c[au]t' /etc/` -> it can be either cat or cut. [] provides a range, like [a-z], [0-9], [abc710] \
`egrep -r 'http[^s]' /etc/` -> negated range, it tells not to look for https \
`egrep -r '/[^a-z]' /etc/` -> no lowercase after / \
32. `tar` can be used to archive files (combining multiple files into a single file). Use `tar -tf file.tar` or `tar tf file.tar` to see the files inside the achived structure, also known as tarball. Tar archive also stores the files permissions of the respective files. \
Exanples: \
`tar --create --file archive.tar file1` or `tar cf archive.tar file1` -> creates a tarball and adds file1 in it \
`tar --append --file archive.tar file2` or `tar rf archive.tar file2` -> appends new files to the tarball \
`tar --extract --file archive.tar` or `tar xf archive.tar` -> will extract the files in the tarball in the current directory \
`tar --extract --file archive.tar --directory /tmp/` or `tar xf archive.tar -C /tmp/` -> will extract the files in the tarball in the tmp directory
33. Compression on linux can be done by gzip, bzip2 or xz. They will compress the files and automatically delete the original files afterwards. Decompression can be done using gunzip, bunzip, unxz. It can also be done using the `--decompress` attribute of the gzip, bzip2 and xz. You can check the files compressed by `gzip --list file1`. These tools like gzip and others can be used on single files. Hence thy can be used together with tar.
34. You can also retain the original files post compression/decompression through the `--keep` attribute like `gzip --keep file1` or `xz -k file1`
35. `zip archive file1` or `zip archive.zip file1` can also be used to compress. `zip -r archive.zip directory/` can zip an entire directory. To unzip use `unzip archive.zip`
36. tar can be used to archive and compress together. \
Examples:
`tar --create --gzip -file archive.tar.gz file1` or `tar czf archive.tar.gz file1` \
`tar --create --bzip2 -file archive.tar.gz file1` or `tar cjf archive.tar.gz file1` \
`tar --create --xz -file archive.tar.gz file1` or `tar cJf archive.tar.gz file1` \
`tar --create --autocompress --file archive.tar.gz file1` or `tar caf archive.tar.gz file1` -> tar will automatically detect to use gzip \
`tar --extract --file archive.tar.gz` or `tar xf archive.tar.gz`