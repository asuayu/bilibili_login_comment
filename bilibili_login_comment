#-*-coding:utf-8-*- 
import requests
try:
    import cookielib
except:
    import http.cookiejar as cookielib
import re
import time
import os.path
try:
    from PIL import Image
except:
    pass
import datetime

# 构造 Request headers
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36'
}

# 使用登录cookie信息
session = requests.session()
session.cookies = cookielib.LWPCookieJar(filename='cookies')
try:
    session.cookies.load(ignore_discard=True)
except:
    print("Cookie 未能加载")

# 获取验证码
def get_vdcode():
    t = str(int(time.time()*1000))
    captcha_url = 'https://passport.bilibili.com/captcha.gif?r=' + t + "&type=login"
    r = session.get(captcha_url, headers=headers)
    with open('captcha.jpg', 'wb') as f:
        f.write(r.content)
        f.close()
    # 用pillow 的 Image 显示验证码
    # 如果没有安装 pillow 到源代码所在的目录去找到验证码然后手动输入
    try:
        im = Image.open('captcha.jpg')
        im.show()
        im.close()
    except:
        print(u'请到 %s 目录找到captcha.jpg 手动输入' % os.path.abspath('captcha.jpg'))
    vdcode = input("please input the captcha\n>")
    return vdcode


def isLogin():
    url = "http://space.bilibili.com/"
    login_res = session.get(url,allow_redirects=False,headers = headers)
    login_code = login_res.status_code
    if int(x=login_code) == 200:
        #print login_res.text
        return True
    else:
        return False


def quick_login(secret, account):
    post_url = 'https://passport.bilibili.com/ajax/miniLogin/minilogin'
    postdata = {
        'pwd':secret,
        'userid':account,
    }
    login_page = session.post(post_url, data=postdata, headers=headers)
    print(login_page.text)
    session.cookies.save()

def login(secret, account):
    post_url = 'https://passport.bilibili.com/login/dologin'
    postdata = {
        'pwd': secret,
        'userid': account,
    }
     # 需要输入验证码后才能登录成功
    postdata["vdcode"] = get_vdcode()
    login_page = session.post(post_url, data=postdata, headers=headers)
    #print(login_page.text)
    session.cookies.save()

try:
    input = raw_input
except:
    pass
def read_message(avid,pageid):
    message_url = 'http://api.bilibili.com/x/v2/reply'
    para = {
        'jsonp':'jsonp',
        'type':'1',
        'sort':'0',
        'oid':avid,
        'pn':pageid,
        'r':'0.19292328566353922',
        'nohot':'1',
        'callback':'jQuery17201519523222946677_1474974409988',
        '_':'1474974619524'
    }
    message = session.get(message_url,params = para,headers = headers)
    if message.status_code ==200:
         json = message.content

         return json

def post_message(message,avid):
    act_url = 'http://www.bilibili.com/video/av517690/'
    act2_url = 'http://api.bilibili.com/x/v2/reply/add'
    postdata = {
        'message':message,
        'oid':avid,
        'jsonp':'jsonp',
        'type':'1',
        'plat':'1',
    }
    #act = session.post(act_url, data=postdata, headers = headers)
    act = session.post(act2_url,data = postdata, headers=headers)
    #time.sleep(1)
    act_code = act.status_code
    if int(act_code) == 200:
        print "评论成功"
    else:
        print "error"

if __name__ == '__main__':
    sendmessage = False
    getmessage = False
    if isLogin():
        print('您已经登录')
    else:
        account = 'account'
        secret =  'secret'
        login(secret, account)
    print('#####################################')
    message = "考古 at "+datetime.datetime.strftime(datetime.datetime.today(), "%Y-%m-%d %H:%M:%S")
    avid = '106'

    av_str = 'av23()av10493()av10388(️)av106()av107()av11425()av12450()av423257()av277610()av872061()av1937120（)av588247()av129073()av508()av62797()av328530()av88888()av88885()av1337495()av39()av10429()av25674()av123688()av129520()av5317()av268364()av1310084()av535903()av564621()av16097()av564624()av570210()av563083()av622070()av622679()av1047954()av882708()av954563()av53065()av53376()av1593037()av1345966()av1594039()'
    pa = r'\d+'
    av_list = re.findall(pa, av_str)
    if sendmessage:
        post_message(message,avid)
    if getmessage:
        json =read_message(avid,1)
        pa = r'"message":"(.*?)",'
        list = re.findall(pa,json)
        print 'av'+avid+' '+datetime.datetime.strftime(datetime.datetime.today(), "%Y-%m-%d %H:%M:%S")
        for i in list:
            print i
    for i in av_list:
        avid = 'av'+i
        print avid
        time.sleep(1)
        #post_message(message, i)
    print('#####################################')
