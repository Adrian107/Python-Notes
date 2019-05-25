 操作模式 | 具体含义                         |
| -------- | -------------------------------- |
| `'r'`    | 读取 （默认）                    |
| `'w'`    | 写入（会先截断之前的内容）       |
| `'x'`    | 写入，如果文件已经存在会产生异常 |
| `'a'`    | 追加，将内容写入到已有文件的末尾 |
| `'b'`    | 二进制模式                       |
| `'t'`    | 文本模式（默认）                 |
| `'+'`    | 更新（既可以读又可以写）         |


[](./Images/file-open-mode.png)


### Read TXT file

Default is 'r'
If 'encoding' is not specified, use system default encoding, may cause the failure of reading file


    def main():
        f = open('致橡树.txt', 'r', encoding='utf-8')
        print(f.read())
        f.close()


    if __name__ == '__main__':
        main()
        
More user-friendly:
        
    def main():
        f = None
        try:
            f = open('致橡树.txt', 'r', encoding='utf-8')
            print(f.read())
        except FileNotFoundError:
            print('无法打开指定的文件!')
        except LookupError:
            print('指定了未知的编码!')
        except UnicodeDecodeError:
            print('读取文件时解码错误!')
        finally:
            if f:
                f.close()


    if __name__ == '__main__':
        main()
        
        
 Besides 'read', 'for-in' and 'readlines':

     import time


    def main():
        # 一次性读取整个文件内容
        with open('致橡树.txt', 'r', encoding='utf-8') as f:
            print(f.read())

        # 通过for-in循环逐行读取
        with open('致橡树.txt', mode='r') as f:
            for line in f:
                print(line, end='')
                time.sleep(0.5)
        print()

        # 读取文件按行读取到列表中
        with open('致橡树.txt') as f:
            lines = f.readlines()
        print(lines)


    if __name__ == '__main__':
        main()

### Write a file

If append, mode = 'a';

    from math import sqrt


    def is_prime(n):
        """判断素数的函数"""
        assert n > 0
        for factor in range(2, int(sqrt(n)) + 1):
            if n % factor == 0:
                return False
        return True if n != 1 else False


    def main():
        filenames = ('a.txt', 'b.txt', 'c.txt')
        fs_list = []
        try:
            for filename in filenames:
                fs_list.append(open(filename, 'w', encoding='utf-8'))
            for number in range(1, 10000):
                if is_prime(number):
                    if number < 100:
                        fs_list[0].write(str(number) + '\n')
                    elif number < 1000:
                        fs_list[1].write(str(number) + '\n')
                    else:
                        fs_list[2].write(str(number) + '\n')
        except IOError as ex:
            print(ex)
            print('写文件时发生错误!')
        finally:  # Execute in any event, whether an exception has occurred or not.
            for fs in fs_list:
                fs.close()
        print('操作完成!')


    if __name__ == '__main__':
        main()
        
### Binary file (Images)
    def main():
        try:
            with open('guido.jpg', 'rb') as fs1:
                data = fs1.read()
                print(type(data))  # <class 'bytes'>
            with open('吉多.jpg', 'wb') as fs2:
                fs2.write(data)
        except FileNotFoundError as e:
            print('指定的文件无法打开.')
        except IOError as e:
            print('读写文件时出现错误.')
        print('程序执行结束.')


    if __name__ == '__main__':
        main()
        
