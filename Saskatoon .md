# Saskatoon 🇨🇦
> City in Saskatchewan, Canada
<br>
This server tests the ability of the user to seperate out a string of text from a log file and write the output to an existing file.

#### Commands used
* `cut`
* `awk`
* `cat`
* `pipe "|"`
* `sort`
* `uniq`
* `tail`


## Brief

<img width="1742" height="645" alt="image" src="https://github.com/user-attachments/assets/bb7f51c7-0bd1-4501-bafb-22b3c0e2abf1" />

### 1. Problem Setup

From the brief - there is a web server access log file `access.log` located in `home/admin/`

The file will have a 

### For this server there are 2 approaches using the `cut` and `awk` command:  

#### 1) Using `cut` command:

To use the `cut` command the syntax is as follows:

```
cut -d "delimiter" -f (field number) file.txt
```

Where

`delimiter` - this will be a space "` `"
`field number` - as the ip address is the first field printed, this is set to 1
`file.txt` - this is the file - the full path to the access.log file is specified

as such the command is:

     admin@ip-172-31-27-155:/$ cut -d " " -f 1 home/admin/access.log

  
#### 2) Using `awk` command:

Before using the `awk` command the file first needs to be concatenated using the `cat` command and then piped into `awk`. The first part of the syntax is:

     cat /home/admin/access.log | 

The syntax for the `awk` command is:

     awk '{ print $1 }'

Where `f` denotes the field

The `awk` command counts strings of non-blank non-line as fields by default

As the ip address is the first string, this is set to 1 - awk uses $ to denote field so this is `$1`.

`print` simply means to write to standard output


The resulting command is:

     cat /home/admin/access.log | awk '{ print f 1 }'

The result is a list of all the ip addresses by themselves

     83.149.9.216
     83.149.9.216
     83.149.9.216
     83.149.9.216
     83.149.9.216
     .
     .
     .
     82.165.139.53
     100.43.83.137
     63.140.98.80
     63.140.98.80
     66.249.73.135
     180.76.6.56
     46.105.14.53

Using the `uniq` command can show duplicates of each ip address there are.

HOWEVER since this counts ADJACENT duplicates the list needs to be sorted first for `uniq` to identify these:

    admin@ip-172-31-27-155:/$ cut -d " " -f 1 home/admin/access.log | sort
     1.22.35.226
     1.22.35.226
     1.22.35.226
     1.22.35.226
     1.22.35.226
     1.22.35.226
     100.2.4.116
     100.2.4.116
     100.2.4.116
     100.2.4.116
     100.2.4.116
     100.2.4.116
     100.43.83.137
     100.43.83.137
     100.43.83.137
     .
     .
     .

The `uniq` command with the `-c` argument then prints the ip addresses along with the count of duplicates for it:

      admin@ip-172-31-27-155:/$ cut -d " " -f 1 home/admin/access.log | sort | uniq -c
      1 1.22.35.226
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      1 100.43.83.137
      .
      .
      .

Using the `tail` instead of `head` function with the `-n 1` argument allows me to see the ip address with the most connections:

     admin@ip-172-31-27-155:/$ cut -d " " -f 1 home/admin/access.log | uniq -c | sort | tail -n 1
     97 75.97.9.59

The file can now be written to the highest ip file using the `echo` command:

     echo "75.97.9.59" > home/admin/highestip.text

     
