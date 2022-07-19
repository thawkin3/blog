# Cleaning up Disk Space on a Linux Machine from the Command Line

I still run an EC2 instance on AWS that I configured years ago to host some of my side projects. It's using a Linux AMI (Amazon machine image) that is woefully out of date and also wonderfully under-performant when traffic on my site is heavy.

Recently my EBS (elastic block store) volume that I have attached to my EC2 instance became full. Not wanting to shell out a trivial amount of cash to upgrade the size of my EC2 instance or EBS volume, I began exploring how I could remove some unnecessary files on my machine that were taking up space.

I'm by no means an expert in Linux. Hence, I probably googled 20 different things searching for solutions that could help me. Stack Overflow, blog posts, and help forums came to my rescue.

Now I'm paying it forward and sharing some commands that helped me.

*Note: These commands work for Ubuntu/Debian Linux distributions. Mileage may vary for other flavors of Linux.*

---

## Checking disk usage and free disk space

To check your machine's disk usage and see how much free space is left, you can run the following command in your terminal:

```
df -h
```

This lists all the filesystems mounted on your machine, where they are located, their size, how much storage space is used, and how much storage space is available.

For me, this reported that my root filesystem was 100% full!

*Source: https://www.howtogeek.com/409611/how-to-view-free-disk-space-and-disk-usage-from-the-linux-terminal/*

---

## Auto-removing unnecessary dependencies

When installing a dependency on a Linux machine, it's common to use APT, the Advanced Package Tool. Over time, it's possible that you may have packages installed that you no longer need. To auto-remove any unnecessary dependencies you may have installed, you can run:

```
sudo apt-get autoremove
```

*Source: https://askubuntu.com/questions/527410/what-is-the-advantage-of-using-sudo-apt-get-autoremove-over-a-cleaner-app*

---

## Cleaning up cached packages

It's also helpful to clean up the package files stored in the `/var/cache` directory on your machine. You can do so by running this command:

```
sudo apt-get clean
```

*Source: https://www.networkworld.com/article/3453032/cleaning-up-with-apt-get.html*

---

## Finding and deleting files with filenames that match a pattern

Back in my early days of using Git, I didn't fully understand the value of the `.gitignore` file, which tells Git to, well, ignore certain files or directories. Because I failed to use this file to my advantage, I had plenty of files like `.DS_Store` or NPM debug logs taking up space on my machine.

To find all the `.DS_Store` files in all subdirectories on my machine, I ran:

```
find . -name ".DS_Store"
```

This lists all the matching files but doesn't do anything with them. It's more of a dry run. If you want to then delete those files, you can run the same command but pass it the `-delete` flag like this:

```
find . -name ".DS_Store" -delete
```

Because these files were also still in my source control due to not listing them in the `.gitignore` file, I also needed to commit and push this change as well as modify my `.gitignore` file to ignore these pesky files.

*Source: https://unix.stackexchange.com/questions/3672/how-do-i-delete-all-files-with-a-given-name-in-all-subdirectories*

---

## Displaying all sub-directories and their sizes, sorted by size

I wanted to see which of my many projects were taking up the most space. To find that list, I ran the following command, which goes through all of the sub-directories from within your current directory, sorts them from largest to smallest, and prints their size in a human-readable format:

```
du -hs * | sort -hr
```

*Source: https://superuser.com/questions/554319/display-each-sub-directory-size-in-a-list-format-using-one-line-command-in-bash*

---

## Listing all files and directories within a directory, sorted by size

Once I identified my largest directories, I needed to dig into which files within those directories were taking up too much space.

You can list all the files and directories within a given directory simply by running this command:

```
ls
```

But, you can also pass additional flags to that command to sort and format the output. You can list all files and directories in a directory sorted by their size and with their size info shown in a human-readable format by using the following:

```
ls -laSh
```

*Source: https://www.tecmint.com/list-files-ordered-by-size-in-linux/*

---

## Searching for large files anywhere on your machine

After I had gone through each directory and cleaned up files I didn't need or deleted projects I was no longer showcasing, I still had quite a bit of disk space being used up. I couldn't easily identify where in my projects I had gone wrong or what was taking up all this space.

This next command was a life saver. This searches for large files over the specified size (50MB in my case) and prints them to the console.

```
sudo find / -type f -size +50M -exec ls -lh {} \;
```

I was surprised at what I found. The two biggest culprits were some cached files from `snapd` that I no longer needed and some pre-allocated storage space from MongoDB I wasn't going to need in this development-like environment.

At this point it was as simple as running `rm <filename>` and `rm -rf <directory name>` to delete my unwanted files and directories.

*Source: https://stackoverflow.com/questions/20031604/amazon-ec2-disk-full/20032145*

---

## Good to Go!

![Success](https://dev-to-uploads.s3.amazonaws.com/i/uwiffjp7slxs7g0jwttx.jpeg)
<figcaption>Photo by Japheth Mast on Unsplash</figcaption>

After troubleshooting for what felt like a few hours, I finally had my disk space usage back to a normal amount, and my EC2 instance was functioning properly again. Success!
