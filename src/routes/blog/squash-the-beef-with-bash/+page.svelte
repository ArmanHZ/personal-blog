
<script lang="ts">
    import Highlight from "svelte-highlight";
    import bash from "svelte-highlight/languages/bash";
    import dracula from "svelte-highlight/styles/dracula";

    const code1 = `# We want to give this frequently used command a shorter name, since we don't want to write -Al all the time.
ls -Al`;
    const code2 = `# Usage: man <command_name>
man ls`;
    const code3 = `# Beware of spaces around =
alias ll='ls -Al'
# And then, you can run
ll`;
    const code4 = `alias ls='ls --color=auto'
alias ll='ls -Al'
alias cls='clear'
alias ..='cd ..'
alias grep='grep -n --color=auto'    # Colored and line number grep
alias ip='ip --color=auto'
alias diff='diff --color=auto'
alias tmux='tmux -f $HOME/.config/Scripts/tmux.conf'    # You can also specify config paths by using aliases`;

    const code5 = "touch functions";
    const code6 = `# Editing the 'functions' file

#!/bin/bash
function first_function() {
    echo I got $# parameters.
    echo First parameter is $1.
}`;

    const code7 = `source functions

first_function Hello World!    # Calling our function with parameters 'Hello' and 'World!'

# Output
I got 2 parameters.
First parameter is Hello.`;

    const code8 = `# Editing the 'functions' file

#!/bin/bash
function first_function() {
    echo I got $# parameters.
    for i in $@
    do
        printf "$i "  # Prints elements without new lines at the end
    done
    echo  # New line just for prettier output
}`;

    const code9 = `source functions
first_function Hello World!

# Output
I got 2 parameters.
Hello World!`;

    const code10 = `$0: This variable holds the name of the script itself, including the path, if provided.
$1, $2, $3, ...: These variables represent the command-line arguments passed to the script. $1 holds the first argument, $2 holds the second, and so on.
$#: This variable stores the number of command-line arguments passed to the script.
$@: It represents all the command-line arguments as a list. This is similar to using "$1", "$2", etc., but allows you to reference all arguments collectively.
$?: After a command is executed, this variable contains the exit status of the last executed command. A value of 0 typically indicates success, while non-zero values indicate errors.
$$: This variable contains the process ID (PID) of the current script or shell.`;

    const code11 = `# Creates a directory and if the creation is successful, navigates to the newly created directory
function mkdircd() {
    mkdir -p "$1" && cd "$1"
}

# fzf + vim. Performs a fzf search for a file and opens it with vim
vif() {
    if [[ $# -gt 0 && $1 == '-h' ]]; then
        file=$(find . 2> /dev/null | fzf)
    else
        file=$(fzf)
    fi
    if [[ $? != 0 ]]; then
        return 0
    else
        vim $file
    fi
}`;

    const code12 = `# Inside your .bashrc add the following line
source $HOME/.config/aliasesrc

# Alternatively, you can use ~ instead of $HOME
# In that case, add this
source ~/.config/aliasesrc`;

</script>

<svelte:head>
  {@html dracula}
</svelte:head>

<div class="main-content">
    <h1>Intro</h1>
    <p>In this guide, I will be going over the following subjects:</p>
    <ol>
        <li><a href="#aliases">Aliases and how to create them</a></li>
        <li><a href="#functions">Functions, parameters and some bash specific syntax</a></li>
        <li><a href="#persistency">Saving everything in a file and making them persistent</a></li>
    </ol>

    <h1 id="aliases">Aliases</h1>
    <p>
        Aliases, as the name suggests, are aliases for other commands! By using aliases you give any built-in (or custom) bash command whatever name you want. The use case for aliases are not only giving different commands different names. You can also set an alias for a complex command which utilizes many flags.
    </p>
    <p>An example is as follows:</p>

    <Highlight language={bash} code={code1} />

    <p>
        The <code>-Al</code> part of the command is called flags. They provide additional functionalities or change the behavior of the commands.
    </p>
    <p>
        In this case, the <code>-A</code> flag is used to show hidden files and <code>-l</code> is for list view.
        Note that <code>-Al</code> is a combination of two flags and it can be represented as <code>-A -l</code> as well.
    </p>
    <p>
        On a side note, you can learn about the flags of any command using <code>man</code>.
    </p>
    <p>
        For the <code>ls</code> example, we have:
    </p>

    <Highlight language={bash} code={code2} />

    <p>
        Getting back to aliases, let's say you want to run <code>ls -Al</code> whenever you type <code>ll</code>.
        You can do this as follows:
    </p>

    <Highlight language={bash} code={code3} />

    <p>
        Congratulations! You have created your first alias. However, if you close your terminal, the alias will be deleted. We will get back to this later on.
        Most popular distributions already provide you with some QoL improving aliases.
    </p>
    <p>
        You can list all of them by running <code>alias -p</code>.
    </p>
    <p>
        Before moving on to the next topic, I will provide you with some of my aliases to hopefully give you inspiration to create your own aliases.
    </p>

    <Highlight language={bash} code={code4} />

    <h1 id="functions">Functions</h1>

    <p>
        Functions are similar to aliases, where you can, for instance, create a function called <code>ll</code> and inside of it call <code>ls -Al</code> and achieve the same result as the previous alias example. However, there is more to functions.
        With functions, you can have parameters or inputs and execute more complex things compared to aliases. Think of it as any other programming language.
    </p>
    <p>
        To write out first function, we first need to create a file and edit it.
    </p>
    <p>
        You can use <code>touch {"<file_name>"}</code> to create an empty file and edit it with your editor of choice. The file's extension is not important.
    </p>
    <p>
        For this example, I will create a file called 'functions'.
    </p>

    <Highlight language={bash} code={code5} />
    <Highlight language={bash} code={code6} />

    <p>
        This function will print out the number of parameters it received, as well as the first parameter.
    </p>
    <p>
        The <code>#!/bin/bash</code> line is a special line called "shebang". It is defined at the beginning of a script file and tells the system to use '/bin/bash' to execute the script.
    </p>

    <p>
        After saving the file, must first source it. You can do it by <code>source {"file_name"}</code>.
    </p>
    <p>
        Taken from the <code>man</code> page, this command lets us "Execute commands from a file in the current shell".
    </p>

    <Highlight language={bash} code={code7} />

    <p>To print every input parameter, we can utilize loops as follows:</p>

    <Highlight language={bash} code={code8} />

    <p>
        Since we changed our function, we have to source our 'functions' file and then call our function.
    </p>

    <Highlight language={bash} code={code9} />

    <p>
        We could have just used <code>echo $@</code> to achieve the same thing, however, I wanted to show you the bash loops.
    </p>
    <p>
        Great! We wrote our first function that takes arguments. Now some bash related info, mainly the special variables.
    </p>
    <p>
        There are many special variables, here are some to get you started:
    </p>

    <Highlight language={bash} code={code10} />

    <p>
        Now, I will provide some of my favorite functions to hopefully inspire you to make your own!
    </p>

    <Highlight language={bash} code={code11} />

    <h1 id="persistency">Persistency</h1>

    <p>
        We learned about aliases and functions. However, when we close our terminal, our aliases go away and for the functions, we need to source them every time.
    </p>
    <p>
        A common method to solve this problem is to create a file called <code>aliasesrc</code> (you can name the file whatever you want. This name is somewhat the norm). "rc" stands for "Run Commands". Again "rc" is the norm and "rc" files are often used to customize the behavior and appearance of software and the user's shell environment.
    </p>
    <p>So, we will stick to the conventions.</p>
    <p>
        After creating your <code>aliasesrc</code> file, you can put all your aliases and functions inside of it.
    </p>
    <p>
        You can place this file where ever you want.
        I tend to place it inside of <code>.config</code> directory, which is in your <code>$HOME</code> directory.
        After you placed the file, it is time to modify your <code>.bashrc</code> file to source our "rc" file when we open the terminal.
        (Note: Depending on the distro, you might have .zshrc or something other than .bashrc)
    </p>

    <Highlight language={bash} code={code12} />

    <p>
        And we are done! Now, every time you open your terminal, your aliases and functions will be ready to run!
    </p>

    <p>Thanks for reading!</p>

</div>