## ������쳣
������쳣�������
>����`Error`�ǳ����п���Ԥ֪����������Ի��������һ����������������úõĴ�������  
>�쳣`Exception`�ǳ������޷�Ԥ֪����������Ի��׳�һ���쳣

�Գ�������Ϊ����  
1.�쳣

    def div(a,b):
        if b == 0:
            raise Exception("����������Ϊ0")  #�����쳣
        return a/b
    
    def cal():
        a = int(input())
        b = int(input())
        div(a,b)
        #�˴�Ϊ���������߼�
    cal()
        #����������ն������뱻����0�������ֱ�����쳣���Ͳ���ִ������Ĵ����߼���

����������쳣
    
    def div(a,b):
        if b == 0:
            raise Exception("����������Ϊ0")  #�����쳣
        return a/b
    
    def cal():
        a = int(input())
        b = int(input())
        try:
            div(a,b)
        except Exception as e:
            print(e)
        #�˴�Ϊ���������߼�
    cal()
    #������ʹ���뱻����0������Ҳ�����ִ�к���Ĵ����߼���ֻ�����һ���쳣����ʾ�ַ���

���԰�����Ĵ������`while`ѭ�����������Ҳ����������
    
    def div(a,b):
        if b == 0:
            raise Exception("����������Ϊ0")  #�����쳣
        return a/b
    
    def cal():
        while 1:
            a = int(input())
            b = int(input())
            try:
                div(a,b)
            except Exception as e:
                print(e)
            #�˴�Ϊ���������߼�
    cal()

2.����
    
    def div(a,b):
        if b == 0:
            return None, "����������Ϊ0"  #����Ǵ���

        return a/b, None
    
    def cal():
        while 1:
            a = int(input())
            b = int(input())
            v,err = div(a,b)   #v��err���ڽ���div����return���������
            if err is not None:  
            #����None��ȫ��Ψһ�Ķ������Ե���None��ֵ��ĳ��ֵ��ʱ��Ҫ��`is`,������`==`
            #�����`=`,�ᱨ��; �����`==`,ֻ�᷵��'None'����ַ���
                print(err)
            else:
                print(v)
    cal()

### ����Щ�������쳣��������������Ҫ���쳣��ʱ��ȴû���쳣���ó������ִ����

    import re
    desc = "age:18"
    a = re.match("ages:(.*)",desc)  #ages�����ﲢ������
    if a is not None:
        print(a.group(1))
    #���ǳ��򲻻����쳣����Ϊ������ʽĬ���ǻ����ƥ�䲻���������Ĭ���ⲻ���쳣
