# 4.2. Использование Python для решения типовых DevOps задач

## 1.
                c = a + b
                TypeError: unsupported operand type(s) for +: 'int' and 'str'
2 подпункт

                #!/usr/bin/env python3
                a = '1'
                b = '2'
                c = a + b
                print (c)
3 подпункт

                #!/usr/bin/env python3
                a = 1
                b = 2
                c = a + b
                print (c)
## 2. 

        #!/usr/bin/env python3

        write_file=set()
        import os
        dir = os.path.abspath(os.curdir)
        bash_command = ["cd"+dir, "git status"]
        print ('&&' .join(bash_command))
        result_os = os.popen(' && '.join(bash_command)).read()
        is_change = False
        for result in result_os.split('\n'):
            if result.find('modified') != -1:
                prepare_result = result.replace('\tmodified:   ', '')
                write_file.add(prepare_result)
        print (write_file)

Вывод программы

        python3 zad2.py
        dir&&git status
        {'new_4.py', 'new_1.py', 'zad2.py'}
## 3.

        #!/usr/bin/env python3
        import sys
        write_file=set()
        import os
        filename =  input ('Вв :')

        print (filename)

        if filename == '':
                dir = os.path.abspath(os.curdir)
                print (dir)
                bash_command = ["cd "+dir, "git status"]
        else:
                dir = os.path.abspath(filename)
                print ('2' + dir)
                bash_command = ["cd "+dir, "git status"]
        result_os = os.popen(' && '.join(bash_command)).read()
        is_change = False
        for result in result_os.split('\n'):
            if result.find('modified') != -1:
                prepare_result = result.replace('\tmodified:   ', '')
                write_file.add(prepare_result)
        print (write_file)
Вывод даной программы
в 1 случае не указан путь (по умолчанию)
2 путь прописан
        python git:(master) ✗ python3 zad3.py
        Вв :

        /home/mikhail/python
        {'zad2.py', 'new_1.py', 'new_4.py'}
        ➜  python git:(master) ✗ python3 zad3.py
        Вв :/home/mikhail/konf
        /home/mikhail/konf
        2/home/mikhail/konf
        {'limits.conf.back'}
## 4.
Код  
 
                #!/usr/bin/python
                import json
                import dns
                
                lasres={}
                with open('result','r') as infile:
                lasres=json.load(infile)
                from dns import resolver
                res={}
                print (lasres)
                with open('/home/mikhail/python/ipad', 'r', encoding='utf-8') as file:
                for m in file.readlines():
                ip = dns.resolver.query(m.rstrip())
                res[m.rstrip()]=(list(map(str,ip)))
                for i in res:
                comm = set(res[i]) & set(lasres[i])
                if set(res[i])-comm != set():
                print('ERROR ',i,'IP mismatch: ',set(res[i])-comm ,set(lasres[i])-comm)
                with open ('result' ,'w') as outfile:
                json.dump(res, outfile)
                print (res)
вывод

                python3 ip-t.py
                {'drive.google.com': ['142.251.1.194'], 'mail.google.com': ['64.233.162.18', '64.233.162.83', '64.233.162.19', '64.         233.162.17'], 'google.com': ['173.194.222.100', '173.194.222.139', '173.194.222.138', '173.194.222.101', '173.194.2         22.102', '173.194.222.113']}
Тест 1 файла (для сравнения)

                ERROR  drive.google.com IP mismatch:  {'64.233.161.194'} {'142.251.1.194'}
                ERROR  google.com IP mismatch:  {'173.194.221.139', '173.194.221.100', '173.194.221.113', '173.194.221.101', '173.1         94.221.138', '173.194.221.102'} {'173.194.222.139', '173.194.222.101', '173.194.222.113', '173.194.222.102', '173.1         94.222.138', '173.194.222.100'}
Тест 2 файл (для сравнения)

                {'drive.google.com': ['64.233.161.194'], 'mail.google.com': ['64.233.162.18', '64.233.162.83', '64.233.162.19', '64         .233.162.17'], 'google.com': ['173.194.221.138', '173.194.221.113', '173.194.221.102', '173.194.221.100', '173.194.         221.101', '173.194.221.139']}
