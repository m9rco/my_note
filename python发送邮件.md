```

# -*- coding: utf-8 -*-

'''

发送报警邮件

'''



import smtplib

from email.mime.text import MIMEText





mailto_list=["13372576987@189.cn"]

mail_host="smtp.aliyun.com"  #设置服务器

mail_user="tt67wq@aliyun.com"    #用户名

mail_pass=""   #口令

mail_postfix="aliyun.com"  #发件箱的后缀







def send_mail(to_list,sub,content):

    me="warehouse"+"<"+mail_user+">"

    msg = MIMEText(content,_subtype='plain',_charset='utf8')

    msg['Subject'] = sub

    msg['From'] = me

    msg['To'] = ";".join(to_list)

    try:

        server = smtplib.SMTP()

        server.connect(mail_host)

        server.login(mail_user,mail_pass)

        server.sendmail(me, to_list, msg.as_string())

        server.close()

        return True

    except Exception as e:

        print(str(e))

        return False







if __name__ == '__main__':

    if send_mail(mailto_list,"hello","有入库失败的东西啊！"):

        print("发送成功")

    else:

        print("发送失败")

```
