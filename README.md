# Automated-Backup-Script
This script automates the backup of files that have been modified within the last 24 hours from a specified directory to a backup location.

## Description
ABC International INC. suffers from a bottleneck where interns manually backup updated password files daily. This script automates the process, reducing human error and improving security.

## Features
Automatic Backup: Backs up files modified within the last 24 hours.
Scheduled Backup: Configured via crontab to run daily.
Compression: Archives files into a compressed .tar.gz format.
Move to Destination: Automatically moves the backup file to a specified destination directory.

# positional parameters
# $0 - name of the script
# $# - number of arguments
# $* - all arguments
# $@ - same as $* but treats each argument as separate
# $1 - first argument
# $2 - second argument

# echo $0
# echo $#
# echo $*
# echo $@
# echo $1
# echo $2

# read from stdin

# echo "Enter your name"
# read name
# echo "Hello $name"

# read from stdin with prompt

# read -p "Enter your name: " name
# echo "Hello $name"

# read from stdin with prompt and hide input

# read -sp "Enter your password: " password
# echo "Password is $password"

# read -p "Enter a number: " num
# if ((num%2==0))
# then echo "even"
# else echo "odd"
# fi
# check if the input is a number


# read -p "Enter a number: " num
# if [[ $num =~ ^[0-9]+$ ]]
# then
#     if ((num%2==0))
#     then echo "even"
#     else echo "odd"
#     fi
# else
#     echo "Not a number"
# fi

# check if the number is an integer

# read -p "Enter a number: " num
# function is_int() {
#     if [[ $1 =~ ^-?[0-9]+$ ]]
#     then
#         return 0
#     else
#         return 1
#     fi
# }

# check odd or even

# if is_int $num 
# then
#     if (($num%2==0))
#     then echo "even"
#     else echo "odd"
#     fi
# else
#     echo "not an integer"
# fi


# keep asking for a number until it is an integer
# read -p "Enter a number: " num
# function is_int() {
#     if [[ $1 =~ ^-?[0-9]+$ ]]
#     then
#         return 0
#     else
#         return 1
#     fi
# }


# # keep asking for a number until it is an integer

# while true
# do
#     read -p "Enter a number: " num
#     if is_int $num
#     then
#         break
#     else
#         echo "not an integer"
#     fi
# done

# # check the number is even or odd

# if (($num%2==0))
# then echo "even"
# else echo "odd"
# fi


# while the input is not end keep asking for a number

# function is_int() {
#     if [[ $1 =~ ^-?[0-9]+$ ]]
#     then
#         return 0
#     else
#         return 1
#     fi
# }

# while [[ $num != "end" ]]
# do
#     read -p "Enter a number: " num
#     if is_int $num
#     then
#         if (($num%2==0))
#         then echo "even"
#         else echo "odd"
#         fi
#     else
#         echo "not an integer"
#     fi
# done


#!/bin/bash

# This checks if the number of arguments is correct
# If the number of arguments is incorrect ( $# != 2) print error message and exit
if [[ $# != 2 ]]
then
  echo "backup.sh target_directory_name destination_directory_name"
  exit
fi

# This checks if argument 1 and argument 2 are valid directory paths
if [[ ! -d $1 ]] || [[ ! -d $2 ]]
then
  echo "Invalid directory path provided"
  exit
fi

# [TASK 1]
targetDirectory=$1
destinationDirectory=$2 

# [TASK 2]
echo "$targetDirectory"
echo "$destinationDirectory"

# [TASK 3]
currentTS=`date +%s`

# [TASK 4]
backupFileName="backup-[$currentTS].tar.gz"

# We're going to:
  # 1: Go into the target directory
  # 2: Create the backup file
  # 3: Move the backup file to the destination directory

# To make things easier, we will define some useful variables...

# [TASK 5]
origAbsPath=`pwd`

# [TASK 6]
cd $destinationDirectory # <-
destDirAbsPath=`pwd`

# [TASK 7]
cd $origAbsPath # <-
cd $targetDirectory # <-

# [TASK 8]
yesterdayTS=$(($currentTS - 24 * 60 * 60))

declare -a toBackup

for file in $(ls) # [TASK 9]
do

  # [TASK 10]
  if ((`date -r $file +%s` > $yesterdayTS))
  then
    # [TASK 11]
    toBackup+=($file)
  fi
done

# [TASK 12]
tar -czvf $backupFileName ${toBackup[@]}
# [TASK 13]
mv $backupFileName $destDirAbsPath
# Congratulations! You completed the final project for this course!
