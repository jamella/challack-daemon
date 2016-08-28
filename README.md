# challack-daemon
challack-daemon adjusting the kernel parameter tcp_challenge_ack_limit by randomizing to prevent side channel attacks

# Abstract
A shortly presented side-channel attack has given strong attention of the community [1] . For lots of servers or smartphone devices this attack considered dangerous for ipv4 connections. There is no doubt that the kernel will get is fixed this issue in the next versions. However, some admins might not update the kernel due to specific reasons or just lazyness. 

Adjusting the parameter to a very high value [2] will work fine. On the other side for server application it might result in an unecessary amount of traffic. To prevent this i have written the challackd(aemon) program. It is able to get parametered to aim the solution presented in [1].

The challack daemon does this job very well and keeping it simple. An former concept of mine was looking for a loadable kernel module but i discarded this, because there is a powerful interface between user- and kernelspace called proc vfs. With proc files we are able to do the job with a simple daemon.

This is my first open-source project with a nice benifit for admins who want to secure agains challenge_ack_limit attacks and are not able to update their kernel. Just compile it and run it on your server.

I need help from the community to make this project "community standard".

# Description

The daemon is coded in c. It maybe compiled with the Makefile (all;debug;clean). make all is compiling all source-files without any debug informations. Maybe you want to know what challack daemon does, do it with make debug. make clean removes only the executable file. The daemon write a random value r in the intervall (base - noise) < r < (base + noise) into the proc file. base and noise can take any positive integer and the default is base=100 noise=20.

# Files

1. *Makefile*      - Makefile
2. *global.h*      - Handle compiler options and define global variables
3. *main.c*        - main function and your entry in challack-daemon
4. *init_daemon.h* - interface header for init_daemon.c
5. *init_daemon.c* - initialization of the daemon, just to make challackd a daemon process
6. *start_daemon.h*- interface header for start_daemon.c
7. *start_daemon.c*- main functionality with random-generator and proc-file Handling
8. *stop_daemon.h* - interface header for stop_daemon.c
9. *stop_daemon.c* - containing the signal handler for SIGUSR1 which is used to stopp the daemon
10. *test.sh*       - simple check the current kernel parameter to show the daemon is working
11. *challackd.script.h* - start stop script of the daemon

# TODOs

Please look for TODOs inside the src files for some things i would like to work on. Feel free to branch in any way you want.
I would like to learn a lot from this project. 

Makefile	-
My wish for the Makefile is to make it a kind of standard with installation, kernel-release queries and so on.

start_daemon	-
Any part of code which might cause the daemon to crash must be fixed

stop_daemon	-
Any part of code which is not a kind of standard must be fixed

init_daemon	-
Any part of code which might caus the daemon to crash must be fixed

main	-
My intention is that main parameters are given as simple integer, currently it works well. However maybe there is a much better way of handling?

# Output 

1 seconds intervall of "sysctl net.ipv4.tcp_challenge_ack_limit"

net.ipv4.tcp_challenge_ack_limit = 222

net.ipv4.tcp_challenge_ack_limit = 227

net.ipv4.tcp_challenge_ack_limit = 191

net.ipv4.tcp_challenge_ack_limit = 178

net.ipv4.tcp_challenge_ack_limit = 229

net.ipv4.tcp_challenge_ack_limit = 167

net.ipv4.tcp_challenge_ack_limit = 189

net.ipv4.tcp_challenge_ack_limit = 229

# Author

Bastian Pukallus, please mail to bastianpukallus@gmail.com

# Sources

[1] http://www.cs.ucr.edu/~zhiyunq/pub/sec16_TCP_pure_offpath.pdf

[2] https://www.mail-archive.com/debian-user@lists.debian.org/msg705042.html
