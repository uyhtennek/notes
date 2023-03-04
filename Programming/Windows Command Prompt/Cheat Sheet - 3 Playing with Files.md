`attrib`
see all the files with attributes and their attributes in the current directory.
it doesn't give any explaination but only give a letter referring what attributes the files have. so make use of `attrib /?` to see all the available attributes.

`attrib +[attribute name] [file name]`
`attribute -[attribute name] [file name]
add or remove attribute `[attribute name]` to the file `[file name]`
can also do like `attribute +r -h [file name]`
___
`del [file]`
delete file
___
`type [file]`
display the content in the file
___
`echo [content] > [file]`
overwrite `[file]` with `[content]`

`echo [content] >> [file]`
append `[content]` to `[file]`

if `[file]` dosn't exist in current directory, it'll create that file for us.
___
`[command] > [file]`
eg. `dir > MyDir.txt`
run the `[command]` and save the result to `[file]` in text form
___
`copy [source] [destination]`
copy files in `[source]` to `[destination]`, but only files, not directories

`xcopy [source] [destination]`
same as `copy`, but with other options and thus functionality
but default the two do the same

`xcopy [source] [destination] /s`
copy files, directories, and sub-directories, except it's empty, in `[source]` to `[destination]`
`xcopy [source] [destination] /e`
copy files, directories, and sub-directories, including empties, in `[source]` to `[destination]`
___
`move [source] [destination]`
move `[source]` to `[destination]`
___
`rename [old name] [new name]`
rename file or directory with the `[old name]` to `[new name]`
___
