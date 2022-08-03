# Vim tutorial and commands 

Vim is a text editor that has 3 most important modes of work: 
 * **normal** --> default; for simple navigation and editing
 * **insert** --> for explicitly modyfing and editing text
 * **command line** --> for saving and editing text 

Main benefits of the VIM command line editor are:
 * fast navigating
 * convenient file editing
 * automatic code highlighting
 * lightweight 

## How to install vim? 

`sudo apt-get install vim`

## How to create and open file with vim? 

`vim <file_name>` 

e.g.

`vim simple_script.py` 

And you will enter simple script with python code highlighting. 

## How to change between different modes in vim? 

`Esc` switches to *normal* mode. 

`i` switches to *edit* mode.

`:` switches to *command line* mode. 

## Navigating through file 

If you want to go on certain line number, which is very important when you're programming, 
enter *command line* mode and go on wanted line. So for example if you want to 
go on line number 7 you will type in following: 
```
:7
```
and press `Enter`. You should be on line 7. 

If you want to go on a last line type in following: 
```
:$
```

Besides from navigating with line numbers, it's possible to use `hjkl` letters 
as arrows. 

```
** To move the cursor, press the h,j,k,l keys as indicated. **
             ^
             k              Hint:  The h key is at the left and moves left.
       < h       l >               The l key is at the right and moves right.
             j                     The j key looks like a down arrow.
             v
```
(taken from `vimtutor`) 

## Basic and most important vim commands 

In order to exit file without saving it enter *command line* mode and type in: 
```
:q!
```
and press `Enter`. 

or you can type in `ZQ`. 

In order to exit and save file, enter *command* line mode and type in: 
```
:wq
```
and press 'Enter'. 

## Text editing 

You can edit text even when not in *insert* mode. 

If you want to delete whole line of text, in normal mode just type `dd` fast enough and 
whole line where the cursor is placed should be deleted. 

If you want to undo last command, just type `u`. 

If you want to copy a line type in `yy` fast enough and whole line where cursor 
is placed will be copied. 

If you want to paste copied stuff, press `p` and stuff will be pasted 
after the cursor. 

## vimtutor cmd

If you want to see all the posibilities of vim and official tutorial, type in 
following command: 
`vimtutor`  

Approximated time to complete `vimtutor` is `25-30` minutes and should be 
enough for you to start using vim for programming.  

## Used links

Most of the information presented here is taken from: 
 * [getting started with vim](https://opensource.com/article/19/3/getting-started-vim) 
 * [how to show line numbers in vim](https://linuxize.com/post/how-to-show-line-numbers-in-vim/)
 * [vim cheat sheet](https://vim.rtorr.com/) 


 
