# 3c.CREATION FOR FILE TRANSFER USING TCP SOCKETS
## AIM
To write a python program for creating File Transfer using TCP Sockets Links
## ALGORITHM:
1. Import the necessary python modules.
2. Create a socket connection using socket module.
3. Send the message to write into the file to the client file.
4. Open the file and then send it to the client in byte format.
5. In the client side receive the file from server and then write the content into it.
## PROGRAM
client.py
import socket

def receive_file(filename, server_socket):
    try:
        # Open the file to write in binary mode
        with open(filename, 'wb') as file:
            while True:
                data = server_socket.recv(1024)  # Receive data from server in chunks
                if not data:
                    # If no data is received, it means the server closed the connection
                    break
                file.write(data)  # Write received data to the file
    except Exception as e:
        print(f"Error while receiving file: {e}")
    finally:
        server_socket.close()  # Ensure the socket is closed

def start_client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # Create TCP socket

    try:
        client_socket.connect(('127.0.0.1', 5555))  # Connect to the server

        filename = input("Enter filename to save: ")
        client_socket.sendall(filename.encode())  # Send the filename to the server

        receive_file(filename, client_socket)  # Receive file from server
        print(f"File '{filename}' received successfully")

    except ConnectionAbortedError:
        print("Connection was aborted by the server.")
    except ConnectionRefusedError:
        print("Could not connect to the server. Ensure the server is running.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        client_socket.close()  # Ensure the client socket is closed

start_client()

server.py
import socket

def send_file(filename, client_socket):
    try:
        # Open the file in binary mode for reading
        with open(filename, 'rb') as file:
            # Send file data in chunks
            for data in file:
                client_socket.sendall(data)  # Send data to the client
        print(f"File '{filename}' sent successfully")
    except FileNotFoundError:
        print(f"File '{filename}' not found")
    except Exception as e:
        print(f"Error while sending file: {e}")
    finally:
        client_socket.close()  # Ensure the client socket is closed after sending

def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # Create TCP socket
    server_socket.bind(('127.0.0.1', 5555))  # Bind the socket to a local address and port
    server_socket.listen(5)  # Listen for incoming connections (up to 5 clients)
    print("Server started, listening on port 5555")

    while True:
        try:
            client_socket, addr = server_socket.accept()  # Accept incoming client connection
            print(f"Accepted connection from {addr}")

            filename = input("Enter filename to send: ")  # Prompt for the file to send
            send_file(filename, client_socket)  # Send the file to the connected client

        except Exception as e:
            print(f"Error: {e}")
        finally:
            client_socket.close()  # Ensure the client socket is closed even in case of an error

start_server()

## OUPUT
![3c client](https://github.com/user-attachments/assets/028919e7-05cb-4745-99ab-2028ef6582e4)
![3c](https://github.com/user-attachments/assets/f69de109-5405-42ea-8532-a6a20a2e385f)


## RESULT
Thus, the python program for creating File Transfer using TCP Sockets Links was 
successfully created and executed.
