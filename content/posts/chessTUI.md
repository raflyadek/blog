+++
date = '2026-10-11T16:10:49+07:00'
draft = false
title = 'ChessTUI'
tags = ['chess', 'terminal']
+++

i love chess. that game is maybe in my top 3 favourite things of all time. and this year i just started tinkering with a terminal, and yeah that thing easily in my top 10. and combining those two into 1 things? that might be the coolest thing ever for me.

<!--more-->
i always love playing chess since i was a kid, i dont know the exact reason, but maybe because my father love chess so much and play it a lot, and it just passed onto me lol. the thing i love about chess is its simplicity yet so complex at the same time. you always know how a knight move, or a bishop move, its been more than 500 years since the last [change](https://coddy.tech/chess/concepts/history-of-chess) happen, and the game is played on 8 x 8 square board, yet there is roughly [10¹²⁰](https://en.wikipedia.org/wiki/Shannon_number) possible game you could play in chess, that is greater than the estimated number of atoms in observable universe which [10⁸⁰.](https://en.wikipedia.org/wiki/Eddington_number) thats crazy number btw.

starting this year, i used terminal a lot, from tinkering with [timer](https://github.com/caarlos0/timer) on terminal, [todo](https://github.com/webstonehq/tuxedo) on terminal, even move to [nvim](https://neovim.io/) code editor that runs directly inside terminal, the first thing i do when i arrive at work is literally just open terminal, thats my whole workflow lol. i dont know man, i just suddenly love everything thats on terminal, or maybe ive had a thing for terminal but just realized it.

i started this project because my current job is not writing code much, and not using [golang.](https://go.dev/) at some point i just feel like my ability to write code kinda rusty, so here we are *rawdogging* a chess engine in a programming language that im not that comfortable with anymore. 

when i started this project, i create a roadmap for myself and a timeline, and just taking it easy to be honest, like first month my goals is literally just create board representation and piece representation. there is an optimal way to create a board representation using [bitboard](https://chessprogramming.org/Bitboards) but because i dont really understand how that works, and yeah i decided to using 2d array approach, because chess is an 8 x 8 board, we can just write a function like this 
{{< highlight go >}}
func initBoard() [8][8]string {
	board := [8][8]string{}

    //initialize piece position
	//white
	board[6] = [8]string{"P", "P", "P", "P", "P", "P", "P", "P"}
	board[7] = [8]string{"R", "N", "B", "Q", "K", "B", "N", "R"}

	//black
	board[1] = [8]string{"p", "p", "p", "p", "p", "p", "p", "p"}
	board[0] = [8]string{"r", "n", "b", "q", "k", "b", "n", "r"}

	return board
}
{{< /highlight >}}

later we can loop that 2d array and use that to represent the board, piece and notation, after that two complete, i can feel my ability to write code *kinda* back, i decided for the next month i want working on legal moves, and rules for each piece. legal moves here is to validate the input moves, for example when i typed "m3", its not legal because notation is only a to h, making sure player cant move opposite color, or eat piece from the same color, etc. rules for each piece is kinda hard, but knight being the easiest is not on my *bingo*, at first i thought knight would be the hardest because knight move is kinda crazy, its L shape move, it can jump the other piece, but that thing just makes validate knight move easier, because we can just do
{{< highlight go >}}
//cant arrive at the same row/col
if fromRow == toRow || fromCol == toCol {
  return fmt.Errorf("invalid knight move")
}
if differenceColAbs == 2 {
  if differenceRowAbs != 1 {
    return fmt.Errorf("thats invalid knight move")
  }
}

if differenceColAbs == 1 {
  if differenceRowAbs != 2 {
    return fmt.Errorf("invalid knight move man")
  }
}
{{< /highlight >}}

now we enter the hardest part, *en passant*, check and checkmate. when i wrote checkmate logic, it makes me crazy fr, because there is a lot of things that you have to check, because the code is kinda long and ugly ill just write the pseudocode here. first get the info how many piece that check our king, if its 1 we can simply search is there any friend piece that can eat that piece but we must search if this friend piece is being pinned by another opponent piece or no if yes then search friend piece that can block the path to our king, if its 2 piece that check our king (discovered check) you have to check what square is available for king to run, after that check if each square is save, if no save square then its checkmate, the pseudocode is somehow sound simple, but the code is a lot of work lol.

making this chess engine project is really fun, genuinely. there is so many things or feature that i want to implement for this project, for example later i want to implement the ui with [bubbletea](https://github.com/charmbracelet/bubbletea) and after that i want to create its own server so i can play with my friend over ssh, or integrate [lichess API](https://lichess.org/api) so i can play online from terminal with lichess account. for anyone who wants to look at the code here is the [source code,](https://github.com/raflyadek/chessTUI) its a mess, so many flaws and so many code that can be optimized, but who cares? as long as you doing what you like, the rest is just optional. 
