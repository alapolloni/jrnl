jrnl
====

*jrnl* is a simple journal application for your command line. Journals are stored as human readable plain text files - you can put them into a Dropbox folder for instant syncinc and you can be assured that your journal will still be readable in 2050, when all your fancy iPad journal applications will long be forgotten.

Optionally, your journal can be encrypted using AES encryption

### Why keep a journal?

Journals aren't only for 13-year old girls and people who have too much time on their summer vacation. A journal helps you to keep track of the things you get done and how you did them. Your imagination may be limitless, but your memory isn't. For personal use, make it a good habit to write at least 20 words a day. Just to reflect what made this day special, why you haven't wasted it. For professional use, consider a text-based journal to be the perfect complement to your GTD todo list - a documentation of what and how you've done it.

In a Nutshell
-------------

to make a new entry, just type

    jrnl yesterday: Called in sick. Used the time to clean the house and spent 4h on writing my book.

and hit return. `yesterday:` will be interpreted as a timestamp. Everything until the first sentence mark (`.?!`) will be interpreted as the title, the rest as the body. In your journal file, the result will look like this:

    2012-03-29 09:00 Called in sick. 
    Used the time to clean the house and spent 4h on writing my book.

If you just call `jrnl`, you will be prompted to compose your entry - but you can also configure _jrnl_ to use your external editor.

Usage
-----

_jrnl_ has to modes: __composing__ and __viewing__. 

### Viewing:

    jrnl -n 10   

will list you the ten latest entries,

    jrnl -from "last year" -to march   

everything that happened from the start of last year to the start of last march. 

### Using Tags:

Keep track of people, projects or locations, by tagging them with an `@` in your entries:

    jrnl Had a wonderful day on the #beach with @Tom and @Anna.

You can filter your journal entries just like this:

    jrnl @pinkie @WorldDomination

Will print all entries in which either `@pinkie` or `@WorldDomination` occurred.

    jrnl -n 5 -and @pineapple @lubricant

the last five entries containing both `@pineapple` __and__ `@lubricant`. You can change which symbols you'd like to use for tagging in the configuration.

> __Note:__ `jrnl @pinkie @WorldDomination` will switch to viewing mode because although now command line arguments are given, all the input strings look like tags - _jrnl_ will assume you want to filter by tag. 

### Smart timestamps:

Timestamps that work:

* at 6am
* yesterday
* last monday
* sunday at noon
* 2 march 2012
* 7 apr
* 5/20/1998 at 23:42

Installation
------------

You can install _jrnl_ manually by cloning the repository:

    git clone git://github.com/maebert/jrnl.git
    cd jrnl
    python setup.py install

or by using pip:

    pip install jrnl

Afterwards, you may want to create an alias in your `.bashrc` or `.bash_profile` or whatever floats your shell:

    alias jrnl="jrnl.py"

### Known Issues

_jrnl_ relies on the `Crypto` package to encrypt journals, which has some known problems in automatically installing within virtual environments.

Advanced usage
--------------

The first time launched, _jrnl_ will create a file called `.jrnl_config` in your home directory.

### .jrnl_config

It's just a regular `json` file:

    {
        journal:        "~/journal.txt",
        editor:         "",
        encrypt:        false,
        key:            ""
        tagsymbols:     '@'
        default_hour:   9,
        default_minute: 0,
        timeformat:     "%Y-%m-%d %H:%M",
    }

 - `journal`: path to  your journal file
 - `editor`: if set, executes this command to launch an external editor for writing your entries, e.g. `vim` or `subl -w` (note the `-w` flag to make sure _jrnl_ waits for Sublime Text to close the file before writing into the journal).
 - `encrypt`: if true, encrypts your journal using AES encryption.
 - `key`: you may store the key you used to encrypt your journal in plaintext here. This is useful if your journal file lives in an unsecure space (ie. your Dropbox), but the config file itself is more or less safe.
 - `tagsymbols`: Symbols to be interpreted as tags. (__See note below__)
 - `default_hour` and `default_minute`: if you supply a date, such as `last thursday`, but no specific time, the entry will be created at this time
 - `timeformat`: how to format the timestamps in your journal, see the [python docs](http://docs.python.org/library/time.html#time.strftime) for reference


> __Note on `tagsymbols`:__ Although it seems intuitive to use the `#` character for tags, there's a drawback: on most shells, this is interpreted as a meta-character starting a comment. This means that if you type
> 
>     jrnl Implemented endless scrolling on the #frontend of our website.
>
> your bash will chop off everything after the `#` before passing it to _jrnl_). To avoid this, wrap your input into quotation marks like this:
> 
>     jrnl "Implemented endless scrolling on the #frontend of our website."
> 
> Or use the built-in prompt or an external editor to compose your entries.

### Encryption

Should you ever want to decrypt your journal manually, you can do so with any program that supports the AES algorithm and the passwords you entered when running _jrnl_ for the first time. Since AES requires keys to be a multiple of 16 characters, passwords will be padded with trailing white spaces before using it to encrypt or decrypt your journal. Sow, if your password is `rosebud` (which I hope it isn't), the key with which to decrypt your journal is `rosebud_________` (the underscores represent whitespaces).

### JSON export

Can do:

    jrnl -json
