# Eggsman
maze game in 10 lines of BASIC

## Purpose

Eggsman is a maze game featuring the eponymous protagonist, the eccentric explorer Xylon Ximenez, whose task is to gather all the eggs in the henhouse before the evil snails crawl over and slime him. Having extracted the eggs he must exit, and then do it all again the next day. Don't ask me why the henhouse is full of killer snails, it just is, OK? It's an execrable existence, and then you expire.

It's a maze with a twist. The eggboxes have turnstiles that Mr Ximenez can barge through but are impervious to snails. Grabbing the magic garlic clove renders snails edible for a little while. But don't step in the poop hazard. Every henhouse has one, directly under the roost.

## How to play

`<`     left  
`>`     right  
`Shift` up  
`Ctrl`  down  

## Mechanics

Squashing the code into 10 lines of BBC BASIC was an exercise involving extraordinary exertions. The game logic required multiple subroutines, but the language allows only one function or procedure definition per line. Also, the maximum extent of each line is 239 characters. I found a solution using self-modifying code. Each of the first four lines of code redefines a function key, which is inserted into the keyboard buffer by a system call at the beginning of line 4. Running the program has the same effect as pressing the function key. The soft key overwrites the line of code that defined it, then runs the program again. The final iteration, encoded on line 0, directly changes the stored code so that the the modified program executes without terminating on line 4. The result is not 10 but 17 lines of code. Note that some of the lines are longer than 239 characters after expansion of the keyword tokens.

```
    0DEFFNr(c)VDU18;-c,M;0;12;D;P*t/w;0;:IFc:=1ELSEt=0:TIME=0:=0
    1DEFFNp(x,y,c,d)=POINT(Q+R*x+Q*c,r*y-Q*d-12)
    2DEFFNd:IFp=0ANDFNq:=1ELSEe=c=0:f=e=0:i=2ANDFNp(a,b,e,f):j=2ANDFNp(a,b,-e,-f):IFi*j:=1-p ELSEc=e:d=f:IFi+j:=j-i ELSE=.5-FNq
    3DEFFNb:e=b*4ANDP:f=(7ANDb)*r-28:n=16ANDRND:FORi=s<0TO0:j=Q-n:VDUC;2,M;e-n;f-j;D;2*n;2*j;C;3,25,69,e;f;:n=j:NEXT:=0
    4A%=131:X%=0:Y%=128:CALL-12:REPEAT:B=31:D=281:G=&A01:h=120:M=&419:P=224:Q=16:R=32:r=-R:T=Y%:T!4=G:FORi=1TOX%:NEXT:VDU278;23;10,R,0;0;0;531;5;0;787;2;0;23,T;0;8,0;0;29,480;832;24,0;-D;P;12;B,19,Q,529;h,h,h:k=3:S=0:v=0:REPEATFORa=0TO62:G?a=1:NEXT:v=v+1
    5V=2^v:w=M*.8^v+D:CLG:FORi=-16TO16STEPR:FORj=1TO3:n=64*j:G?(i/Q*j?&87+B)=0:VDU18;3,M;5*R*j-T;i-&8C;D;n-96;0;D;0;8*i;D;P;0;M;n+i+Q;3*n-412;D;0;r;M;R*j+R;i-12-n;D;R;0;:NEXT,:G?B=0:FORj=2TO3:REPEATi=RND(61):UNTILG?i=1:G?i=j:NEXT:s=r:FORb=1TO54STEP5:E=b+FNb:NEXT
    6REPEATa=0TO62:C=786+FNi:NEXT:T!7=&30F1464:REPEATs=0:u=B:x=3:y=4:Z=0:m=FNr(FNh(FNs(Q))):REPEATCOLOUR3:c=0:d=0:REPEATi=INKEY-103-INKEY-104:j=INKEY-1-INKEY-2:IFi c=i:d=0:IF0ELSEIFj d=j:c=0:IF0ELSEUNTILTIME>t+9:t=TIME:p=FNp(x,y,c,d):IFFNh(c+d=0ORp=1)GOTO9ELSEFORe=0TO1
    7REPEATj=-1TO0:IFp=2IFc=2*e-1ORd=2*j+1b=8*x+y+8*e+j:IFb MOD5=4IFFNb ELSENEXT,:x=(x+c+7)MOD7+FNh(0):y=y+d:u=x+7*y:a=u:q=G?a:G?a=FNh(FNi):IFq S=S+q?A%+FNs(q?M):IFq>2s=(q-2)ORs ELSEIFq=2m=3+FNr(0)ELSEE=E-q:IFq>E a=B+20*SGNRND+FNs(27):G?a=4+FNi:i=FNi
    8X%=HIMEM:Z%=Z%MOD3+1:FORz=0TOZ:IFz !T=!FNy:IFFNc IFv>3ORz=Z%q=p=0:p=o+V<8:?A%=p*q*(o+V):q=q+q*p:?T=(a-q*c+7)MOD7+7*(b-q*d):g!FNw=!T:FORj=Y TOT-4STEP4:i=1ANDg<>j ANDg?3=j?3AND?g=?j:g?3=g?3ORi:j?3=j?3ORi:NEXT:IFFNw+q IFFNc n=SGNFNd:g?1=n*c+3:g?2=n*d+3
    9NEXT:IFZ<m+3IFFNr(w>t)=0Z=Z+1:m=FNw:!Y=&3021F+512ANDRND:IF0ELSEVDU275;1+m;0;B,3855;4:PRINTS*10:UNTILs:u=1ANDs:SOUND0,-9*u,7,3:k=k-u:VDUB,21-k,Q,R:FORi=1TOG:NEXT:FORz=FNh(0)TOZ:G?B=FNw:NEXT:i=k<1:UNTILi ORs-u:UNTILi:VDUB,2574;4:COLOUR2:IFS>H%H%=S:PRINT"HISCORE":IF0ELSEUNTIL0
   10DEFFNw:Y=T-4*Z+FNy:IF1ANDo g?3=o-1:=0ELSEVDUC;SGNz,M;R*a+4*o*c;r*b-4*o*d;5,64:=0
   11DEFFNy:g=T-4*z:b=?g DIV7:a=?g-7*b:c=g?1-3:d=g?2-3:o=g?3:=g
   12DEFFNh(b)VDUC;2,M;R*x;r*y;5,h:h=h EORb*r:VDU8,-b*h,C;1+b,25;0;20;46:=b
   13DEFFNi:b=G?a:VDUC;1-(b=1),M;a MOD7*R;a DIV7*r;5,b?&407:=0
   14DEFFNs(b)FORi=4ANDb TO7ANDb:SOUND0,-9,b/8EORi,1:NEXT:=0
   15DEFFNq:i=x-a:j=y-b:=RND(1)<.5+SGN(i*c+j*d)*-1^m/(1+ABS(i*d+j*c))
   16DEFFNc:p=2ANDFNp(a,b,c,d):IFu<>?g-(o>1)*(c+7*d)ANDu-?g:=1ELSEIFm S=S+20:g!FNw=!Y:z=Z:Z=Z-1:=FNs(7)ELSEs=1ORs:=FNs(23)
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
`Z` total number of snails on screen (max 3)  
`@%` system variable governing print format  
`A%` constant 131  
`B%` constant holding data on maze item characters  
`X%` high score  
`Y%` number that determines the length of the delay before the game restarts  
`Z%` counter that determines which ghost is about to move  
