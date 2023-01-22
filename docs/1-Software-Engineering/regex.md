# Regex

Regex are in simple terms "a sequence of characters that define a search pattern"

- basic is `*.gif` where * means anything.
- `Position` of search
  - `^` - starts with - `^Simple` - any line that start with this
  - `$` - ends with - `park$` - any line that end with this
- `Frequency` of occurrence
  - `*` - 0 or more - `ab*` a followed by zero or more b's
  - `+` - 1 or more - `ab+` - the letter a followed by one or more b's
  - `?` - 0 or 1 - ab? - zero or just one b
  - `{n}` - n exactly - ab{2} - the letter a followed by exactly two b's
  - `{n,}` - n or more - ab{2,} - the letter a followed by at least two b's
  - `{n,y}` - n to y occurance - ab{1,3} - the letter a followed by between one and three b's
- `Class` of characeters - list or range, and are case-sensitive
  - Range - `[a-z]` or `[A-Z]` or `[0-9]`
  - List - `[a,d,p,w]` or `[adpwKRM]` - match any single character
  - Mix - `[A-Z2-4pw]` - matches range A-Z, 2-4 and literally p, and literally w
  - Except - `[^abXyP-Q]` and character except what is in
- `Flags` to search for special types of characters without needing to list them in a range
  - `.` m any character
  - `\s` - whitespace - `\S` opposite
  - `\w` - word - `\W` not a word
  - `\d` - digit (number) - `\D` not a digit
  - `\b` - word boundary - `ate\b` finds ate at end of word, eg, plate but not gates.- `\B` not boundary
- Other
  - `\n` `\r` `\t` `\0` - new line, carriage, tab, null
  - `|` - or - `The (?:cat|dog) jumps` matches cat or dog
- VS Code - to use what you found in replace use `$0` or `$1`, eg, find `(^Todo)`, replace `<b>$0</b>`. Replaces `Todo` with `<b>Todo</b>`

## Snippets

- mulitline with one line `\s\n+` `\n`
- remove end whitespaces `\s+\n` `\n`

- Remove single line comments // from code:

  - `\/\/.*$\n` - Finds all single line comments starting with //.
    - `\/\/` - string has //
    - `.*` - then has anything after that
    - `$\n` - then matches next line as well.

## Links

- Check and Validate on [Regex101](https://regex101.com/)

