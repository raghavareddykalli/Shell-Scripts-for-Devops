# **Shell script to find and remove the log files older than 30 days in the /var/log folder**

```
find /var/log -type f -name *.log -mtime +30 -exec rm -f {} \;
```

## **Detailed Explanation**

**🔍 Breakdown of the command:**

```find```: The Linux command to search for files in a directory hierarchy.

```/var/log```: The target directory that contains log files.

```-type f```: Limits the search to files (not directories). Use d to search directories

```-name *.log```: Search for files ending with .log 

```-mtime +30```: Filters files modified more than 30 days ago.
                  +30 means strictly older than 30 days.
                  -30 would mean newer than 30 days.

