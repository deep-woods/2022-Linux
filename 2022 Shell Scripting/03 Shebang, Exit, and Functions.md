# Shebang, Exit, and Functions

<br>

### `#!/bin/bash`

- `bash`: Bourne Again Shell
- `dash`: Debian Alquimist shell

There are differences between various versions of shells and some code that runs in one version may not run in another.

`Shebang` is the path to the `bash` shell. Leave the `shebang` path at the head of your `shell` code, and the script will be interpreted by `bash`, thus avoiding shell breaks arising from shell type incompatibility.

<br>

## Exit Code

<br>

### Exit code syntax: `echo $?`

<br>

After running an original command, the exit code stores the exit status of that command. For instance:

    echo $?

- `0`: code exited without problem
- `1`: error occurred. 
- `127`: many other issues...

<br>

It is a good practice to use the `exit code` to store and later use the status value. As best practice, always return the exit codes in your script for debugging. 

    if [ $operation_status = 'fail' ]
    then 
        operation_debug
        exit 1
    fi

<br>
<br>

## Functions

<br>

### When to use functions?

- Installing packages
- Adding users
- Configuring firewalls
- POerforming mathematical calcaultions

<br>

Function syntax: `function func_name() { code }`

<br>

    function add() {
        echo $(( $1 + $2 ))
    }

    sum=$( add 10 58)


<br>

    function add() {
        return $(( $1 + $2 ))
    }

    add 45 20
    sum=$?
    echo $sum

    bash add.sh
    65

<br>

- `return` statement in `shell` is used only to return the `exit status`

<br>

### Best practice

- Modularise code into re-usable units of functions
- Avoid duplicates in code
- Use arguments/parameters to pass in variables. 


<br>

## Hands-on practice

1. Use `for` loop to access each directory and create a file in it. 

- `function func_name() { code }`

        function create_app_dir() {
        mkdir app
        cd app
        mkdir db images utils

        for dir in "$app"/*
        do
            echo $(dir)/logs    <---- Not working!   
        done
        }

        create_app_dir
        
<br>

2. Capture the output to use it as a variable.

        function add(){
            sum=$(( $1 + $2 ))
            echo $sum
        }

        result=$(add $1 $2)
        echo "The result is $result"

<br>

3. Basic Calculator.

        #!/bin/bash
        
        function read_numbers(){
            read -p "Enter Number1: " number1
            read -p "Enter Number2: " number2
        }

        while true
        do
            echo "1. Add"
            echo "2. Subtract"
            echo "3. Multiply"
            echo "4. Divide"
            echo "5. Quit"

            read -p "Enter your choice: " choice

            case $choice in
                1)
                    read_numbers
                    echo $(( $number1 + $number2 )) ;;
                
                2)  read_numbers
                    echo $(( $number1 - $number2 )) ;;
                
                3)  read_numbers
                    echo $(( $number1 * $number2 )) ;;
                
                4)  read_numbers
                    echo $(( $number1 / $number2 )) ;;

                5) break

            esac
        done

<br>
<br>
<br>

# Tips for shell scripting

1) `VIM` EDITOR

- highlights code in different colours

<br>

2) `Shellcheck`

    apt-get install shellcheck

    shellcheck my_code.sh

    In my_code.sh line 10:
      read -p "What is your favourite tree?: " fav_tree
      ^--^ SC2162: read without -r will mangle backslashes.


https://gist.github.com/nicerobot/53cee11ee0abbdc997661e65b348f375
https://github.com/koalaman/shellcheck

<br>

## IDEs

1) `PyCharm`
2) `VS Code`
3) Google Shell Style Guide https://google.github.io/styleguide/shellguide.html


<br>
<br>

## References

  - Kodekloud https://kodekloud.com/