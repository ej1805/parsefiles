# Examples Folder

This folder contains a couple of example files which were encrypted with the
`parsefiles` program as described in the project.

All the original plaintext files are also saved with `.plain` suffix for
reference. Files are encrypted with the password being the filename itself.


Note that while encrypting files `-o` flag was used which shows actual master
key which was used to encrypt all of the files.

```bash
$ make all
# https://www.gutenberg.org/ebooks/2264
curl -s https://www.gutenberg.org/cache/epub/2264/pg2264.txt | tee macbeth.txt.plain > macbeth.txt
echo -n macbeth.txt | ./parsefiles -e -o macbeth.txt
{
    "macbeth.txt": "6502964e2f130260d0ee32c563eb956dd0bdc72a35445c9995b3ac955137bc3e"
}
# https://www.gutenberg.org/ebooks/17996
curl -s https://www.gutenberg.org/files/17996/17996-0.txt | tee aeschylus.txt.plain > aeschylus.txt
echo -n aeschylus.txt | ./parsefiles -e -o aeschylus.txt
{
    "aeschylus.txt": "db8659fa54f3d48e40794adc3dcde8eba5c1e8190904dbb3c8bbef846ec7c156"
}
# https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation
curl -s https://upload.wikimedia.org/wikipedia/commons/f/f0/Tux_ecb.jpg | tee ecb.jpg.plain > ecb.jpg
echo -n ecb.jpg | ./parsefiles -e -o ecb.jpg
{
    "ecb.jpg": "8343dc263137310e712630a1f6604cedecf021c0d08d9ef9016ea4d6741509d8"
}

echo -n wrongpassword | ./parsefiles -s crickets
aeschylus.txt: password does not match
ecb.jpg: password does not match
macbeth.txt: password does not match
no files to search with matching password
make: [Makefile:17: search] Error 1 (ignored)

echo -n macbeth.txt | ./parsefiles -s haha
aeschylus.txt: password does not match
ecb.jpg: password does not match
['haha'] were not found in any of the files

echo -n macbeth.txt | ./parsefiles -s crickets
aeschylus.txt: password does not match
ecb.jpg: password does not match
macbeth.txt

echo -n macbeth.txt | ./parsefiles -s cric*
aeschylus.txt: password does not match
ecb.jpg: password does not match
macbeth.txt

echo -n aeschylus.txt | ./parsefiles -s άδραστου
ecb.jpg: password does not match
macbeth.txt: password does not match
aeschylus.txt

echo -n aeschylus.txt | ./parsefiles -s άδρασ*
ecb.jpg: password does not match
macbeth.txt: password does not match
aeschylus.txt
```
