# Shell Scripting

[Writing Robust Shell Scripts](http://web.archive.org/web/20110314180918/http://www.davidpashley.com/articles/writing-robust-shell-scripts.html)

arrays
```bash
indexedArray=(one two three)
associativeArray=([one]=one [two]=two [three]=three)
indexedArray[0] # one
indexedArray[@] # one two three
```

conditionals
```bash
if [[ condition ]]; then
elif [[ condition2 ]]; then
else [[ condition3 ]]; then
fi
```

arguments
```bash
$1 # first argument, $n is indicative of the nth argument
$@ # all arguments
${@:2} # second argument onwards
```

case statements
```bash
echo -n "Enter the name of a country: "
read COUNTRY

echo -n "The official language of $COUNTRY is "

case $COUNTRY in

  Lithuania)
    echo -n "Lithuanian"
    ;;

  Romania | Moldova)
    echo -n "Romanian"
    ;;

  Italy | "San Marino" | Switzerland | "Vatican City")
    echo -n "Italian"
    ;;

  *)
    echo -n "unknown"
    ;;
esac
```

check if a command exists
```bash
[ ! command -v <command> &> /dev/null ] || echo "missing <command>"
```

check if file exist/not exist
```bash
# if file not exists
[[ ! -f /path/to/file ]] && echo "whoops"

# if file exists
[[ ! -f /path/to/file ]] || source /path/to/file
```

read silently
```bash
read -sp "Password: " password
# then access with
$password
```
# General

symbols 
```bash
%1 # process that was just started and backgrounded
```

## zsh

hash
```bash
hash -d # Display all hash directories
hash -d  shortcut=/path/to/dir  # Create a new hash
~shortcut # Voila :)
```

file sourcing step
```
.zshrc > .zshenv > .zprofile (requires login shell)
```

keybinds
```bash
"^@" set-mark-command
"^A" beginning-of-line
"^B" backward-word "^D" delete-char-or-list
"^E" end-of-line
"^F" forward-word
"^G" send-break
"^H" backward-delete-char
"^I" expand-or-complete
"^J" accept-line
"^K" kill-line
"^[l" clear-screen
"^M" accept-line
"^N" down-line-or-history
"^O" accept-line-and-down-history
"^P" up-line-or-history
"^Q" push-line
"^R" history-incremental-search-backward
"^S" history-incremental-search-forward
"^T" transpose-chars
"^U" kill-whole-line
"^V" quoted-insert
"^W" backward-kill-word
"^X^B" vi-match-bracket
"^X^F" vi-find-next-char
"^X^J" vi-join
"^X^K" kill-buffer
"^X^N" infer-next-history
"^X^O" overwrite-mode
"^X^U" undo
"^X^V" vi-cmd-mode
"^X^X" exchange-point-and-mark
"^X*" expand-word
"^X=" what-cursor-position
"^XG" list-expand
"^Xg" list-expand
"^Xr" history-incremental-search-backward
"^Xs" history-incremental-search-forward
"^Xu" undo
"^Y" yank
"^[^D" list-choices
"^[^G" send-break
"^[^H" backward-kill-word
"^[^I" self-insert-unmeta
"^[^J" self-insert-unmeta
"^[^L" clear-screen
"^[^M" self-insert-unmeta
"^[^_" copy-prev-word
"^[ " expand-history
"^[!" expand-history
"^[\"" quote-region
"^[\$" spell-word
"^['" quote-line
"^[-" neg-argument
"^[." insert-last-word
"^[0" digit-argument
"^[1" digit-argument
"^[2" digit-argument
"^[3" digit-argument
"^[4" digit-argument
"^[5" digit-argument
"^[6" digit-argument
"^[7" digit-argument
"^[8" digit-argument
"^[9" digit-argument
"^[<" beginning-of-buffer-or-history
"^[>" end-of-buffer-or-history
"^[?" which-command
"^[A" accept-and-hold
"^[B" backward-word
"^[C" capitalize-word
"^[D" kill-word
"^[F" forward-word
"^[G" get-line
"^[H" run-help
"^[L" down-case-word
"^[N" history-search-forward
"^[OA" up-line-or-history
"^[OB" down-line-or-history
"^[OC" forward-char
"^[OD" backward-char
"^[P" history-search-backward
"^[Q" push-line
"^[S" spell-word
"^[T" transpose-words
"^[U" up-case-word
"^[W" copy-region-as-kill
"^[[1;5C" forward-word
"^[[1;5D" backward-word
"^[[1~" beginning-of-line
"^[[200~" bracketed-paste
"^[[2~" overwrite-mode
"^[[3~" delete-char
"^[[4~" end-of-line
"^[[A" up-line-or-history
"^[[B" down-line-or-history
"^[[C" forward-char
"^[[D" backward-char
"^[_" insert-last-word
"^[a" accept-and-hold
"^[b" backward-word
"^[c" capitalize-word
"^[d" kill-word
"^[f" forward-word
"^[g" get-line
"^[h" run-help
"^[l" down-case-word
"^[n" history-search-forward
"^[p" history-search-backward
"^[q" push-line
"^[s" spell-word
"^[t" transpose-words
"^[u" up-case-word
"^[w" copy-region-as-kill
"^[x" execute-named-cmd
"^[y" yank-pop
"^[z" execute-last-named-cmd
"^[|" vi-goto-column
"^[^?" backward-kill-word
"^_" undo
" "-"~" self-insert
"^?" backward-delete-char
"\M-^@"-"\M-^?" self-insert
```

## bash/sh

source
```bash
# so bash does not have source it uses . instead
. /path/to/env # not source /path/to/env
```
