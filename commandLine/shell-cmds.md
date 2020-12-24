## basics

ls
cd
cd ..

pwd

ls -l

mkdir
mv name.txt destination

<!-- Downloading -->
curl 'URL'

curl -L 'URL'

curl -o output.txt -L 'URL'

<!-- Viewing files -->
cat
less

<!-- Removing files -->
rm, rmdir

<!-- Searching and pipes -->
grep: "global regular expression print,” processes text line by line and prints any lines which match a specified pattern
wc: "short for word count" reads either standard input or a list of files and generates one or more of the following statistics: newline count, word count, and byte count


grep shell dictionnary.txt | less
(grep for shell in dictionnary.txt and pipe it to less)

curl -L 'URL' | grep fish | wc -l

curl -L 'URL' | grep -c fish
(download the content of the URL and look for fish and count how many words there are)

<!-- customize bash profile file -->
https://friendly-101.readthedocs.io/en/latest/bashprofile.html

Instructor Notes
For users on Windows OS and using Git Bash you won't have a .bash_profile file. Don't worry I will explain to you how to create it.

1) If you're not in the home directory, change into it.

2) Create a file using touch .bashrc

3) Then edit it with Vim vim .bashrc

5) Save and close the file typing :wq and hit Enter.

6) Restart your Git Bash shell.

7) When you open your Git bash it will create a .bash_profile for you.

<!-- customize the bash prompt -->
http://bashrcgenerator.com/



<!-- alias -->
Allows you to create short names for commands
alias ll='ls -la'
