import os
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication

# 定义表白的话
template = """
亲爱的{name}：

    我们认识已经有{time}了，这段时间里，我发现你是一个{evaluation}的女孩，让我感到很开心。我一直想告诉你，我喜欢你，喜欢你的笑容、喜欢你的温柔、喜欢你的一切，喜欢和你在一起的感觉。我知道我们之间有很多特殊的经历，这些经历让我更加坚定了对你的感情。

    我想和你在一起，一起走过未来的每一天。不知道你是否愿意成为我的女朋友呢？

    爱你的{name}
"""

# 从命令行读取女孩的名字、认识时间、评价和特殊经历
name = input("请输入女孩的名字：")
time = input("请输入你们认识的时间（例如：3个月、半年）：")
evaluation = input("请输入你对她的评价（例如：可爱、温柔）：")
experience = input("请输入你们之间的特殊经历：")

# 根据模板生成表白的话
love_letter = template.format(name=name, time=time, evaluation=evaluation, experience=experience)

# 将表白的话保存到文件
filename = "love_letter.txt"
with open(filename, "w") as f:
    f.write(love_letter)

print("表白的话已保存到文件中。")

# 读取保存的文件，并输出表白的话
if os.path.exists(filename):
    with open(filename, "r") as f:
        print(f.read())
else:
    print("文件不存在。")

# 通过电子邮件发送表白的话
email_address = input("请输入你的电子邮箱地址：")
email_password = input("请输入你的电子邮箱密码：")
to_address = input("请输入女孩的电子邮箱地址：")

msg = MIMEMultipart()
msg['From'] = email_address
msg['To'] = to_address
msg['Subject'] = '我喜欢你'

body = MIMEText(love_letter)
msg.attach(body)

# 附件
with open(filename, 'rb') as f:
    attachment_part = MIMEApplication(f.read())
    attachment_part.add_header('Content-Disposition', 'attachment', filename=filename)
    msg.attach(attachment_part)

try:
    server = smtplib.SMTP_SSL('smtp.qq.com', 465)
    server.login(email_address, email_password)
    server.sendmail(email_address, to_address, msg.as_string())
    server.quit()
    print("表白的话已通过邮件发送给女孩。")
except Exception as e:
    print("邮件发送失败：", e)
