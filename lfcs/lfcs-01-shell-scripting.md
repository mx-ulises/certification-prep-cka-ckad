## Essentials shell scripting components

A shell script is a number of commands that are executed sequentially. They work with variables to react differently in different environments. It uses enviromental variables. They can contain as well conditional statements such as if or while. You can use the shell to interpret code from a shell script. If the script only uses internal commands, it will run very fast as it doesn't need to load anything else. These script are not requried to be compatible and no modules are loaded, and this make them statics. They are also not idempotent.

These are some essential componenets of the shell scripts:
 * shebang: goes in the first line and indicates the shell to use. Example:
   * `#!/bin/bash`
 * Code: Code within the script is executed in a subshell
 * Positional parameters: they are specified in the command line an they are referred within the script as `$n`
 * `read`: can be used to store values provided while running the script
 * `exit`: this can be used to provide information about how an script finalized its execution.

Example:

```
#!/bin/bash
# Comments about the script

# Multiline cat
cat << BLAH
what directory?
BLAH

read DIR # Save input value in the variable DIR

cd $DIR
pwd
ls

exit 0 # Diferent exit code will help to identify what type of error the script run int.
```

If you use `source script.sh` you will run the script in the same shell and not in a subshell.

## Pattern matching

The purpose of a pattern matching operator is to clean up a string:
 * `${1#}` prints the string lendth of `$1`
 * `${1#string}` removes the shortest match of the `string` from the front end of `$1`
 * `${1##string}` removes the longest match of the `string` from the front end of `$1`
 * `${1%string}` removes the shortest match of the `string` from the back end of `$1`
 * `${1%%string}` removes the longest match of the `string` from the back end of `$1`

## Conditional statements

 * `if` .. `then` .. `fi`
    ```
    if [ -z $1 ]
    then
        echo "you have to provided an argument"
        exit 6
    fi

    echo the argument is $1
    ```
 * `while` .. `do` .. `done`
    ```
    COUNTER=$1
    COUNTER=$(( COUNTER * 60 ))

    minusone() {
        COUNTER=$(( COUNTER - 1))
        sleep 1
    }

    while [ $COUNTER -gt 0 ]
    do
        echo "You still have $COUNTER seconds left
        minusone
    done
    ```
 * `until` .. `do` .. `done`
 * `case` .. `in` .. `esac`
 * `for` .. `in` .. `do` .. `done`
