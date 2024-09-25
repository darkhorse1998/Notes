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
