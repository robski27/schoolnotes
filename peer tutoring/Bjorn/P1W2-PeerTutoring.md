# Peer Tutoring Week 2

**NODIG**: Debian VM met sudo rechten
#### Objective:
To practice basic Linux commands, explore their functionalities using the man command, and solve open-ended tasks independently.
#### Instructions:
-   Complete all tasks on the Debian-based Linux system you installed last week.
-   Use the man command to explore additional options for each command.
-   Document your steps and observations in a text file.
----------

## Linux Commands Assignment

## Part 1: Navigating Directories

1. **Check current directory:**
   ```bash
   pwd
   ```
   - Output: Prints the current working directory path.

2. **List contents:**
   ```bash
   ls
   ls- l    # Lists files with detailed information (permissions, size, etc.).
   ls - a   # Lists files with detailed information (permissions, size, etc.).
   ```

3. **Create directories:**
   ```bash
   mkdir Linux_Practice
   cd Linux_Practice
   mkdir Sub1 Sub2
   ```

4. **Move between directories:**
   ```bash
   cd Sub1
   cd ..
   ```

5. **Remove directories:**
   ```bash
   rmdir Sub2
   ```

## Part 2: File Management

1. **Create files:**
   ```bash
   touch file1.txt file2.txt
   ```

2. **Copy file1.txt to Sub1:**
   ```bash
   cp file1.txt Sub1/
   ```

3. **Move and rename file2.txt to renamed_file2.txt in Sub1:**
   ```bash
   mv file2.txt Sub1/renamed_file2.txt
   ```

4. **View file contents:**
   ```bash
   cat file1.txt
   less file1.txt
   ```

5. **Delete renamed_file2.txt:**
   ```bash
   rm Sub1/renamed_file2.txt
   ```

## Part 3: Solve with Commands

1. **Concatenate .txt files:**
   ```bash
   cat Sub1/*.txt > combined.txt
   ```

2. **Create a file with random numbers:**
   ```bash
   for i in {1..100}; do echo $((RANDOM % 1000 + 1)); done > random_numbers.txt
   ```

3. **Recovering a file from Trash:**
   ```bash
   mv ~/.local/share/Trash/files/important.txt ./Linux_Practice/
   ```

## Part 4: Exploring Commands with man

### 1. `ls` Command
   - **Useful Options:**
     - `ls -l`: Provides detailed information about files and directories.
     - `ls -a`: Shows all files, including hidden files.

   - **Description:**
     The `ls` command is used to list the contents of a directory.

### 2. `cp` Command
   - **Useful Options:**
     - `cp -r`: Recursively copies directories.
     - `cp -i`: Prompts for confirmation before overwriting files.

   - **Description:**
     The `cp` command is used to copy files and directories.

## Bonus Challenge

1. **Creating a symbolic link:**
   ```bash
   ln -s file1.txt symlink_to_file1.txt
   ```

## Challenges and Findings
- Learning to use `man` effectively provided insights into various command options.
- Creating a random number file using built-in tools
- Navigating the Trash directory