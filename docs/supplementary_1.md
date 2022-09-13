## Escaping 

Escaping is a method of quoting single characters. The escape `(\)` preceding a character tells the shell to interpret that character literally.

!!! warning "Warning"
    With certain commands and utilities, such as `echo` and `sed`, escaping a character may have the opposite effect - it can toggle on a special meaning for that character.

??? info "Special meanings of certain escaped characters"

    <center>

    |Character |  Meaning                                |
    |:---------|:--------------------------------------------|
    |`\n`      | newline                              |
    |`\r`      | return                          |
    |`\t`      |tab                             |
    |`\v`      | vertical tab                          |
    |`\b`      |backspace | 
    |`\a`      | alert (beep or flash) |
    |`0xx`     | translates to the octal ASCII equivalent of 0nn, where nn is a string of digits |

    </center>

???+ info "Special Characters"

    | Character  | Meaning            |
    |:-----------|:-------------------|
    | `#`        | **Comments**. Lines beginning with a `#` (with the exception of `#!`) are comments and will not be executed.| 
    | `;`        | **Command separator [semicolon]**. Permits putting two or more commands on the same line.|
    | `;;`       | **Terminator in a case option [double semicolon]**.|*
    |`"`         |**"partial quoting [double quote]**. "STRING" preserves (from interpretation) most of the special characters within *STRING*| 
    | `'`        |**full quoting[single quote]** 'STRING' preserves all special characters within *STRING*. This is a stronger form of quoting than *"STRING"*|
    | `,`        |**comma operator**. The comma operator [1] links together a series of arithmetic operations. All are evaluated, but only the last one is returned.|
    | `,, ,`     |**Lowercase conversion** in parameter substitution |
    |`\`         |**escape [backslash]**. A quoting mechanism for single characters.|
    |`/`         |**Filename path separator [forward slash]**. Separates the components of a filename.This is also the division arithmetic operator.|
    |**`**       |**command substitution**. The **command** construct makes available the output of command for assignment to a variable. This is also known as backquotes or backticks.|
    |`:`         |**null command [colon]**. This is the shell equivalent of a "NOP" (no op, a do-nothing operation). It may be considered a synonym for the shell builtin true|
    |`!`         | reverse (or negate) the sense of a test or exit status [bang]. The ! operator inverts the exit status of the command to which it is applied|
    |`?`            |**test operator**. Within certain expressions, the ? indicates a test for a condition.|
    |`$`         | **Variable substitution** (contents of a variable).|
    |`${}`        |**Parameter substitution**|
    |`$?`        | **Exit status variable**|
    |`$$`        | **process ID variable|
    