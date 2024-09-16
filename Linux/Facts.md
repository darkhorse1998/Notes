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
7. 