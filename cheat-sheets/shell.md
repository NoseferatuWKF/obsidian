## Programming
arrays
```bash
indexedArray=(one two three)
associativeArray=([one]=one [two]=two [three]=three)
indexedArray[0] # one
indexedArray[@] # one two three
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

## Utils
yes
```bash
yes | some-shell-script.sh # will pass Y to all stdin
yes -A | some-shell-script.sh # will pass A to all stdin
```

stow
```bash
stow --adopt -nv $HOME <stowed-dir> # dry-run and verbose
stow --adopt -v * && git restore . #adopt and sync everything with repo
```

## zsh
hash
```bash
hash -d # Display all hash directories
hash -d  shortcut=/path/to/dir  # Create a new hash
~shortcut # Voila :)
```
