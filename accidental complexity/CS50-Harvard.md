# HarvardX CS50x
https://learning.edx.org/course/course-v1:HarvardX+CS50+X/home

## Lecture 0
binary digit -> bit
binary

millions of tiny switches (transistors)
-> combination of bulbs

8bit -> 1byte

A (65) > assign a number to every letter
! ASCII (American Standard Code for Information) 255 characters

add more bit

Unicode 32bit 4billion possible permutation

RGB pixel

MIDI, mp3, AAC for sound


input -> BLACK BOX -> output
! (abstraction) : to the simplification of something, don't focus on the lower
! algorithm : implementation detail

O(n) O(log2n)


## Lecture 1
C coding

소수점의 계산
컴퓨터는 1 / 10을 정확하게 계산하지 못한다.
나눈 결과를 소수점 50자리까지 출력하기로 하면
소수점 자리로 계속 내려가보면 근사치로 나타난다는 것을 확인할 수 있다.
비트가 유한하기 때문이다.


## Lecture 2
compiling의 4단계
preprocessing. compiling. assembling. linking

compiling
입력한 코드가 -> assembly code

assembling
assembly code가 object code로 번역

linking
machine code는 0과 1의 모음이 결합되는 것이 컴퓨터를 나타내는 언어

cryptography
readability는 cryptography와 관련있다.
plaintext -> cipher -> ciphertext

encrypt <-> decrypt

## Lecture 3

linear search -> binary search

Big O
The running time is at most.

Omega
The running time is at least.

Theta
The running time is on average.

## Lecture 4

### hexadeciaml
2개의 16진수는 1byte의 2진수로 변환된다.

full red. full green. full blue = white (255, 255, 255) FFFFFF
no red. no green. no blue = black (0, 0, 0) 000000
red (255, 0, 0) FF0000
green (0, 255 0) 00FF00
blue (0, 0, 255) 0000FF

### address
inside of your computer’s memory has unique address

### pointer
변수의 adrress를 저장한다.

pointer는 우편함과 같다.
우편함에는 어떤 유형의 파일을 저장한다.
집/회사 주소와 같이 컴퓨터 메모리에도 주소가 있다.

### typedef
C 언어에서는 string이라는 자료형이 없어서
별도로 포인터를 정의하고 배열의 순서를 설정하는 작업이 필요하다.

### Memory
malloc
변수의 memory allocation을 정의한다.
정해진 크기 만큼 메모리를 heap 영역에 할당한다.

### heap overflow
stack overflow라고도 한다.
memory 용량이 heap 영역을 넘어간다.

### virtual memory
실행 프로그램 메모리의 크기에 따라 자동으로 확보되는 여유 메모리 공간

### valgrind grant
memory leak과 buffer overflow를 확인한다.
메모리 누수가 발생하면 시스템 성능이 낮아진다.

### file I/O
fopen 함수에서 r은 읽기, w는 쓰기, a는 덧붙이기 모드를 불러온다.
fclose 함수로 작업을 종료한다.


## Lecture 5

linked list, queue, stack, trie, hash table, dictionary

## Lecture 6
Python

## Lecture 7
CSV (comma separated values)

SQL (Structed Query Language)
B-Tree (Binary Tree)

## Lecture 8
HTML, CSS, Javascript

internet primer
- ip address

DHCP (Dynamic Hosts Configuration Protocol)
DNS
Access Point

## Lecture 9
Flask

## Lecture 10
Emoji

## Lecture 11
Cyber security

Encryption

Cookie

OAuth

VPN

