Before getting stuck into the tricky stuff, there is some easy search/replaces you can do that will help:

- Remove any `;` at the end of lines, e.g. use regular expression find `;\s*$)` and replace with empty string.
- Remove instance methods `-`, e.g. regular expression find `^\s*)-\s*` and replace with `$1func `.

Also remove any `*`'s because Swift doesn't need them to identify reference vs. value types.  This is hard to do safely with a regular expression because of the obvious confusion with the multiplication operator.  So do it manually and with care.
