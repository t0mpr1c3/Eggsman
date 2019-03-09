# Eggsman
maze game in 10 lines of BASIC

## Purpose

* This is a Pacman-type game with turnstiles written for the javascript BBC micro model B emulator, [jsbeeb](https://bbc.godbolt.org/).

* The concept is loosely based on a spider-themed maze game released for the Commodore 64 in the 1980s, whose name I have forgotten. I wrote up a [Hallowe'en style version of the game](https://www.youtube.com/watch?v=i2zMBxIZGmY) for [*BEEBUG* Magazine](http://bbcmicro.co.uk/game.php?id=602) when I was 15 years old. It was published in 1987.

* It was written as an entry for the [2019 BASIC 10 liner contest](http://gkanold.wixsite.com/homeputerium/kopie-von-basic-10liners-2019) in the category WILD. It does not comply with the rules for the EXTREME-256 category because it contains self-modifying code.

## Theme

Eggsman is a maze game featuring the eponymous protagonist, the eccentric explorer Xylon Ximenez, whose task is to gather all the eggs in the henhouse before the evil snails crawl over and slime him. Having extracted the eggs he must exit, and then do it all again the next day. Don't ask me why the henhouse is full of killer snails, it just is, OK? It's an execrable existence, and then you expire.

It's a maze with a twist. The eggboxes have turnstiles that Mr Ximenez can barge through but are impervious to snails. Grabbing the magic garlic clove renders snails edible for a little while. But don't step in the poop hazard. Every henhouse has one, directly under the roost.

The snails start out moving very slowly, because they are snails, but once they get the scent of human blood they start charging around in lethal fashion at much higher speeds. Unfortunately the demands of multiple snails on a 6502 processor make gameplay at the higher levels considerably less responsive, and the speed tops out at level 4. The laws of physics apply even to murderous gastropods.

## How to play

Download EGGWILD.zip from the [github repo](https://github.com/t0mpr1c3/Eggsman) and extract the virtual disk EGGWILD.ssd. Upload the file into *jsbeeb*, or an offline emulator such as [BeebEm](https://en.wikipedia.org/wiki/BeebEm), and type `CHAIN "EGGWILD"`. 

(Note that *jsbeeb* has a strange keyboard mapping: on my US keyboard I got the double quote by typing `SHIFT-2`.)

Alternatively, copy/paste the BASIC code into the console of *BeebEm* and type `RUN`.

### Keys

`X` left  
`C` right  
`J` up  
`N` down  

## Mechanics

### Code summary

Squashing the code into 10 lines of BBC BASIC was an exercise involving extraordinary exertions. The game logic required multiple subroutines, but the language allows only one function or procedure definition per line. Also, the maximum extent of each line is 239 characters. I found a solution using self-modifying code. Each of the first four lines of code redefines a function key, which is inserted into the keyboard buffer by a system call at the beginning of line 4. Running the program has the same effect as pressing the function key. The soft key overwrites the line of code that defined it, then runs the program again. The final iteration, encoded on line 0, directly changes the stored code so that the the modified program executes without terminating on line 4. The result is not 10 but 17 lines of code. Note that some of the lines are longer than 239 characters after expansion of the keyword tokens.

```
0DEFFNc:p=2ANDFNp(a,b,c,d):IFu<>?g-(o>1)*(c+7*d)ANDu-?g:=1ELSEIFm S=S+20:g!FNw=!Y:z=Z:Z=Z-1:=FNs(7)ELSEs=1ORs:=FNs(23)
1DEFFNh(b)VDUC;2,M;R*x;r*y;5,h:h=h EORb*r:VDU8,-b*h,C;1+b,25;0;20;46:=b
2DEFFNb:e=b*4ANDP:f=(7ANDb)*r-28:n=16ANDRND:FORi=s>=0TO0:j=Q-n:VDUC;2,M;e-n;f-j;D;2*n;2*j;C;3,25,69,e;f;:n=j:NEXT:=0
3DEFFNp(x,y,c,d)=POINT(Q+R*x+Q*c,r*y-Q*d-12)
4A%=131:X%=0:Y%=128:CALL-12:REPEAT:B=31:C=786:D=281:G=&A01:h=120:M=&419:P=224:Q=16:R=32:r=-R:T=128:T!4=G:T!7=&30F1464:M!1=&2171F10:FORi=1TOY%:NEXT:VDU278;23;10,R,0;0;0;531;5;0;787;2;0;23,T;0;8,0;0;29,480;832;24,0;-D;P;12;B,19,Q,529;h,h,h:k=3:v=0
5S=0:REPEATCLG:FORu=0TO62:G?u=1:NEXT:FORi=-16TO16STEPR:FORj=1TO3:n=64*j:G?(i/Q*j?&87+B)=0:VDU18;3,M;5*R*j-T;i-&8C;D;n-96;0;D;0;8*i;D;P;0;M;n+i+Q;3*n-412;D;0;r;M;R*j+R;i-12-n;D;R;0;:NEXT,:G?B=0:v=v+1:V=2^v:w=M*.8^v:FORj=2TO3:REPEATi=RND(61):UNTILG?i=1:G?i=j        
6:NEXT:s=r:FORa=0TO62:t=FNi:NEXT:FORb=9TO54STEP5:E=b+FNb:NEXT:REPEATs=0:x=3:y=4:Z=0:m=FNr(FNh(FNs(Q))):REPEATCOLOUR3:c=0:d=0:REPEATi=INKEY-67-INKEY-83:j=INKEY-70-INKEY-86:IFi c=i:d=0:IF0ELSEIFj d=j:c=0:IF0ELSEUNTILTIME>t+9:t=TIME:p=FNp(x,y,c,d):IFFNh(c+d=0ORp=3)GOTO8
7FORe=0TO1:FORj=-1TO0:IFp=2IFc=2*e-1ORd=2*j+1b=8*x+y+8*e+j:IFb MOD5=4IFFNb ELSENEXT,:x=(x+c+7)MOD7:y=y+d:u=x+7*y:a=u:q=G?a:G?a=FNh(FNi):IFq S=S+q?A%+FNs(q?M):IFq>2s=(q-2)ORs ELSEIFq=2m=3+FNr(0)ELSEE=E-q:IFq>E a=B+20*SGNRND+FNs(9):G?a=4+FNi:i=FNi
8Y%=HIMEM:Z%=Z%MOD3+1:FORz=0TOZ:IFz !T=!FNy:IFFNc IFv>3ORz=Z%q=p=0:p=o+V<8:?A%=p*q*(o+V):q=q+q*p:?T=(a-q*c+7)MOD7+7*(b-q*d):g!FNw=!T:FORj=Y TOT-4STEP4:i=1ANDg<>j ANDg?3=j?3AND?g=?j:g?3=g?3ORi:j?3=j?3ORi:NEXT:IFFNw+q IFFNc n=SGNFNd:g?1=n*c+3:g?2=n*d+3
9::NEXT:IFZ<m+3IFFNr(w>t)=0Z=Z+1:z=Z:!FNy=&3001F+512*RND(2):m=FNw:IF0ELSEVDU4,B,3855;:PRINTS*10:UNTILs:u=1ANDs:SOUND0,-9*u,7,3:k=k-u:VDUB,21-k,Q,R:FORi=1TOG:NEXT:FORz=FNh(0)TOZ:G?B=FNw:NEXT:i=k<1:UNTILi ORs-u:UNTILi:VDU4,B,2575;:COLOUR2:IFS>X%X%=S:PRINT"HISCORE":IF0ELSEUNTIL0
10DEFFNw:g=FNy:Y=T-4*Z:IF1ANDo g?3=o-1:=0ELSEVDUC;SGNz,M;R*a+4*o*c;r*b-4*o*d;5,64:=0
11DEFFNq:i=x-a:j=y-b:=RND(1)<.5+SGN(i*c+j*d)*-1^m/(1+ABS(i*d+j*c))
12DEFFNr(c)VDU18;-c,M;0;12;D;P*t/w;0;275;1+m;0;:IFc:=1ELSEt=0:TIME=0:=0
13DEFFNy:g=T-4*z:b=?g DIV7:a=?g-7*b:c=g?1-3:d=g?2-3:o=g?3:=g
14DEFFNs(b)FORi=4ANDb TO7ANDb:SOUND0,-9,b/8EORi,1:NEXT:=0
15DEFFNi:b=G?a:VDUC;1-(b=1),M;a MOD7*R;a DIV7*r;5,b?&407:=0
16DEFFNd:IFp=0ANDFNq:=1ELSEe=c=0:f=e=0:i=2ANDFNp(a,b,e,f):j=2ANDFNp(a,b,-e,-f):IFi*j:=1-p ELSEc=e:d=f:IFi+j:=j-i ELSE=.5-FNq
```

Even allowing self-modification, condensing the code to 10 lines was exorbitantly exasperating and the solution exploits some exotic expertise. For example: Keyword tokens are abbreviated wherever possible. Every subroutine is defined as a function: global variables are preferred to arguments. The variables `A%`, `X%`, and `Y%` that define the system call are reused in the body of the program. The variables `B`, `G`, `M` and `T` are used as both data store and location pointer. The counter `Z%` is used without initialization. The lack of an `ENDIF` statement necessitated some tortuous conditional logic. And so on and so forth. The most time-consuming part was the endless code-juggling to fit no more than 239 bytes into each line. The final steps were exhausting: it took as long to get from 12 lines of code to 11 as were expended to get from 30 lines down to 12, and getting from 11 lines to 10 was exponentially more exacting.
   
Despite the excruciating exigencies of extruding every last byte from the source code, I made essentially no cuts to the game itself. The only exemption that I found expedient to expunge was to excise extra excreta (poop hazards) from higher levels of the game. I expressed especial exuberance at retaining the fairly complex functions `FNd` and `FNq` that determine which way the snails move.
   
## Functions

`FNb` draw gate at grid location `b`  
`FNc` check whether snail has hit man  
`FNd` determine direction of snail  
`FNh` draw man  
`FNi` draw maze item at location `a`  
`FNp` get pixel value in front of man/snail  
`FNq` determine probability of snail direction  
`FNr` draw timer  
`FNs` make sound  
`FNw` draw snail  
`FNy` get snail data  

## Variables

`a` x location of ghost/grid location of object  
`b` y location of ghost/grid location of gate  
`c` intended movement in x direction  
`d` intended movement in y direction  
`e` temporary variable  
`f` temporary variable  
`g` location of data for snail whose turn it is to move  
`h` character depicting man  
`i` temporary variable  
`j` temporary variable  
`k` number of lives that remain  
`m` mode: 0=normal, 3=magic  
`n` temporary variable  
`o` number of pixels moved from square by snail  
`p` temporary variable  
`q` temporary variable  
`s` game state: 0=playing, 1=dead, 2=level up, 3=level up and dead, -32=not playing  
`t` time at last user input   
`u` grid location of man (except line 9)   
`v` game level  
`w` delay between snails (units of 0.01 second)  
`x` x location of man  
`y` y location of man  
`z` index of snail whose turn it is to move  
`B` constant 31  
`C` constant &312  
`D` constant &119  
`E` number of eggs that remain to be collected  
`G` constant &A01  
`M` constant &419   
`P` constant &E0 (width of maze in pixels)  
`Q` constant 16  
`R` constant 32  
`S` game score  
`T` constant 128  
`V` number of pixels each snail moves, per turn  
`Y` location of data for last snail  
`Z` total number of snails on screen (max 4)  
`@%` system variable governing print format  
`A%` constant 131  
`B%` constant holding data on maze item characters  
`X%` high score  
`Y%` number that determines the length of the delay before the game restarts  
`Z%` counter that determines which ghost is about to move  
