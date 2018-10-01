# shell-mysql

# -*- coding: UTF-8 -*-

import datetime
import os

def get_week_player_number(o_day, s_day):
    mysql_path = '/usr/local/webserver/mysql/bin/mysql  --socket=/data/log/run/mysql.sock'
    user = 'root'
    pwd = '123456'
    dbname = 'log'
    command = "SELECT date_format(times,'%Y-%m-%d') as '日期',count(distinct(userid)) as '登陆玩家数量', count(*) as '玩家登录总次数' FROM user_log where action='login' and times> '" + str(s_day) +" 00:00:00' and times<'" + str(o_day) + " 00:00:00'group by date_format(times,'%Y-%m-%d');"

    subject = '一周登录玩家数量和玩家登录总次数'
    toname = 'jacob@netws.com'


    conn = mysql_path + ' ' + '-u' + user + ' ' + '-p' + pwd + ' '+ dbname +' ' + '-e' + '"' + command + '"'
    # print conn
    p = os.popen(conn).read()

    os.system('python /data/shell/mail.py %s %s "<pre><h3><font face="SimSun"> %s </font></h3></pre>" ' %(toname, subject, p))

if __name__ == '__main__':
    # 每周一获取上周的数据
    today = datetime.date.today()
    seven_day = today + datetime.timedelta(today.weekday()-7)
    one_day = today + datetime.timedelta(today.weekday())
    get_week_player_number(one_day, seven_day )
