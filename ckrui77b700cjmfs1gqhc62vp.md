## Basic Octave and MATLAB

This notes are valid for both Octave and MATLAB  
% for comments.  


### CALCULATOR

```
5+4
5-3
2*7
8/3
2^6
log(2)
exp(3)
abs(-1)   %absolute value
floor(1.9)   %round with cut to int (=1)
ceil(1.9)   %round nearest int (=2)
``` 


### LOGICS OPERATIONS
``` 
1 == 2     %returns 0 (false)
1 ~= 2     %~= means different: returns 1 (true)
1 && 0   %AND
1 || 0       %OR
xor(1,0)  %OR alternative
``` 

### VARIABLES
``` 
a=3
b='ciao';   %with ; no output shown
a = 3, b = 'ciao'   %with , more command in the same row
a = 3; b = 'ciao';   %same without print output
c = pi;   %pi number
a   %print "a = 3"
disp(a)   %print the value ("3")
disp(sprintf('%0.2f', c))   %round to second decimal
disp(sprintf('2 decimals: %0.2f', c))   %output: "2 decimals: 3.14"
format long   %format I will see the variables
c   % output: "c = 3.1415926358979"
format short
c   % output: "c = 3.1416"
``` 

### VECTORS AND MATRICES
``` 
v = [1;2;3]    %column vector, matrix 3*1 (separator ;)
w = [1 2 3]   %row vector, matrix 1*3 (separator space or ,)
y = 1:6   %6 rows vector, from 1 to 6
z = 1:0.1:2   %elements from 1 to 2 by 0.1 [1 1.1 ... 2]
length(v)   %elements of vector v
A = [1 2; 3 4; 5 6]   %3*2 matrix, first row 1 2
ones(2,4)   %2*4 matrix of 1
2*ones(5,6)   %5*6 matrix of 2
zeros(1,6)   %6 elements row vector of 0
rand(2,3)   %2*3 matrix with random numbers
randn(2,3)   %2*3 matrix with random numbers from normal distribution
x = randn(1,100);
hist(x)    %hiatogram of x (gaussian)
eye(5)   %identity matrix 5*5 (1 on diagonal, 0 others)
help(eye)   %function guide
size(A)   %vector with matrix A dimensions
length(A)   %matrix A bigger dimension (3)
A(2,2)   %element in row 2 column 2
A(3,:)   %all row 3 elements
A(:,2)   %column 2 elements
A([1 3],:)   %rows 1 and 3
A(:,2) = [8;7;6]   %assingn 8, 7 and 6 to A second column
A = [A, [8,9,10]]   %new column to A
A(:)   %all A elements in a single vector
A = [1 2; 3 4; 5 6];
B = [11 12; 13 14; 15 16];
C = [A B]   %new matrix with A and B side by side
C = [A; B]   %new matrix with A above and B below
D = [7 8; 9 10]
A * D   %multiply the matrices
A .* B   %every A element multiplied with the correspondent of B
A .^ 2   %square every A element
1 ./ A   %invert every v element
log(A)   %log every element
exp(A)   %exp every element
abs(A)   %absolute value of every element
-A   %change sign of every element ( -1*A)
A+1   %increment by 1 every element
A'   %chenage rows and columns (transpose matrix)
max(A)   %max each column
max(A,[],2)   %max each row
max(max(A))   %element max
A < 3   %matrix of true or false
find(A<2)   %position of element less then 3
[r,c] = find(A>2)   %vector r with element row e c with element column > 2
sum(A)   %sum column
sum(A,2)   %sum row
prod(A)   %multiply column elements
prod(A,2)   %multiply row
sum(sum(D .* eye(2))   %sum diagonal elements
flipud(A)   %invert A (last row now first row)
pinv(A)   %inverse matrix
``` 

### WORKSPACE
``` 
pwd   %current directory
cd 'C:\directory'   %move current directory
ls   %file and directory in current dir
addpath('C:\work')   %add directory
load doc.txt   %load file
load('doc.txt')   %same
who   %workspace variables
whos   %same with descriptions
doc   %see file doc loaded before
clear   %clean workspace
save doc2.txt c   %c saved in doc2.txt (with comment)
save doc2.txt c -ascii   %c saved in doc2.txt (only variable)
save doc2.mat c   %c saved in doc2.mat (MATLAB compressed binary file)
clear c   %delete c
load doc2.mat   %load variable c from file
``` 

### GRAPHS
``` 
x = [1,2,3]
y = [4,5,6]
plot(x,y)
hold on   %to preserve the graph and not overwrite it with the next one
z = [7,8,9]
plot(x,z)   %with "hold on" the next is with the next one (same axis)
xlabel('asse x')
ylabel('asse y')
legend('y','z')
title('Grafico 1')
print -dpng 'prova.png'   %export in a png
figure(1); plot(x,y)
figure(2); plot(x,z)   %two separate graphs
subplot(2,2,1)   %create a 2x2 plot page and use the first one
plot(x,y)
subplot(2,2,2)   %now on the right
plot(x,z)
axis([0 10, 0 10])   %change min and max of my axis
clf   %clean graph
A = [1 2 3; 1 3 4; 1 4 5; 1 5 1]
imagesc(A)   %matrix on graph: different numbers equal to different colors
imagesc(A), colorbar   %bar with legend
imagesc(A), colorbar, colormap gray   %gray scale
``` 

### IF, FOR, WHILE
``` 
v = [1,2,3,4,5]

if v(5) == 4,
    v(5) = v(5) + 1;
elseif v(5) == 5,
    v(5) = v(5) - 1;
else
    disp('wtf?');
end;

for i = 1:4,
  v(i) = 2*i;
end;

i = 1;
while i <= 3,
      v(i) = 10;
      i = i+1;
end;

i = 1;
while true,
     v(i) = 100;
     i = i+1;
     if i == 4,
          break;
     end;
end;
``` 

### FUNCTIONS
``` 
function f = quadrato(x)
   f = x^2;
end;
quadrato(3)   %returns 9

function [g1,g2]  = quadratocubo(x)
   g1 = x^2;
   g2 = x^3;
end;
[a,b] = quadratocubo(3)   %returns a = 9, b = 27
``` 

I can save a function in a .m file and use it without load the file if it's in the working directory.