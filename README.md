# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
## Client
```
import socket

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(1)

print("Server is waiting for connection...")

conn, addr = s.accept()
print("Connected:", addr)

total = int(conn.recv(1024).decode())
expected = 1

for i in range(total):
    frame = conn.recv(1024).decode()
    seq = int(frame)

    if seq == expected:
        print("Received Frame:", seq)
        conn.send(str(seq).encode())
        print("Acknowledgement sent:", seq)
        expected += 1

conn.close()
s.close()
```

## Server
```
import socket

s = socket.socket()
s.connect(('localhost', 8000))

total = int(input("Enter total number of frames: "))
window = int(input("Enter window size: "))

s.send(str(total).encode())

i = 1

while i <= total:
    print("\nSending frames:")

    for j in range(i, min(i + window, total + 1)):
        print("Frame sent:", j)
        s.send(str(j).encode())

    for j in range(i, min(i + window, total + 1)):
        ack = s.recv(1024).decode()
        print("Acknowledgement received:", ack)

    i += window

s.close()
```
## OUPUT
<img width="933" height="362" alt="Screenshot 2026-11-02 134850" src="https://github.com/user-attachments/assets/57116cef-faa7-4481-9bb3-de475faebfb2" />

## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
