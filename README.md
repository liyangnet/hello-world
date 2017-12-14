# hello-world

import socket
import threading
# 目的  主线程专门用于接收屏幕输入 --> 发送 --> 退出
# 创建一个子线程 专门用于接收数据
def send_msg(udp_socket):
    data = input("请输入你想说的话：")
    IP = input("请输入你聊天的IP地址：")
    port = int(input("请输入对方使用的端口:"))
    udp_socket.sendto(data.encode(), (IP, port))
def recv_msg(udp_socket):
    while True:
        data, remote_address = udp_socket.recvfrom(4096)
        print("接收到来自%s的数据:%s" % (str(remote_address), data.decode()))
def main():
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    udp_socket.bind(('', 8888))
    # 创建一个子线程收数据
    recv_thd = threading.Thread(target=recv_msg, args=(udp_socket,))
    recv_thd.start()
    while True:
        op = input("请输入你要进行的操作：1 发送数据 2 接收数据 3 退出")
        if op == "1":
            send_msg(udp_socket)
        # elif op == "2":
        #     recv_msg(udp_socket)
        elif op == "3":
            break
        else:
            print("你输入有误！！")
    udp_socket.close()
if __name__ == '__main__':
    main()
