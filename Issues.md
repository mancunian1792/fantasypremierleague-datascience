Issue #1
========

While checking the source of any webpage - Directly Inspect element of Value visible on Webpage isnt always helpful

Thanks to http://www.gregreda.com/2015/02/15/web-scraping-finding-the-api/
Realization 
User(Client) Requests using Browser
Reaches the Server
Data can be fetched and injected into the Template and send as a Response back to the Client
i.e. Server side Page creation
OR
Client side Web App - 
Server sends the Static content HTML page
but Data -  Javascript in the server response fetches the data from an API and uses it to create the page client-side

To get such data - Developer tools - Network - XHR (instead of normal HTML )

#html_doc = urllib2.urlopen("https://fantasy.premierleague.com/drf/bootstrap-static").read()
#print(html_doc[1])

When u make a HTTP Request, if we read it using urllib
It treats the response as a String
Hence despite the format was in JSON, Errors like 
1. TypeError: string indices must be integers, not str
2. AttributeError str-object-has-no-attribute

Hence use other library that treats Request - Response as Json 

Issue #2
========

names.append(full_name.str())
AttributeError: 'unicode' object has no attribute 'str'

Solution - str(full_name)

Issue #3
========

    names.append(str(full_name))
UnicodeEncodeError: 'ascii' codec can't encode character u'\xe9' in position 1: ordinal not in range(128)

Solution
s = full_name.encode('ascii', 'ignore').decode('ascii')

Disadv - removes all other internationalizations

', u'ElliotEmbleton', u'DeclanRice']
To get rid of u'
Convert Unicode string to normal ascii string
str(variable)

  response = requests.get("https://fantasy.premierleague.com/drf/entry/"+main_user_id+"/event/"+gameweek+"/picks")
TypeError: cannot concatenate 'str' and 'int' objects

Solution
str(main_user_id)

 if player["is_captain"]==true:
NameError: name 'true' is not defined
  if player["is_captain"]===true:                             ^
SyntaxError: invalid syntax
Solution
if player["is_captain"]:

TypeError: 'NoneType' object has no attribute '__getitem__'
At times some data depends on User Login - Such data is not returned in JSON response as visible in Postman or HTTP request's corr. response

However visible in Browser->Network->XHR->response part
To know what data was sent while making such a request - rightclick - Get cURL -
for e.g.
For - https://fantasy.premierleague.com/a/team/3146257/event/19

Actual URL was

curl 'https://fantasy.premierleague.com/drf/bootstrap-dynamic' -H 'Host: fantasy.premierleague.com' -H 'User-Agent: Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:50.0) Gecko/20100101 Firefox/50.0' -H 'Accept: */*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'X-Requested-With: XMLHttpRequest' -H 'Referer: https://fantasy.premierleague.com/a/team/3146257/event/19' -H 'Cookie: _ga=GA1.2.309538033.1473872047; __gads=ID=1e26f8a1f937797f:T=1476526048:S=ALNI_MYwwjDM8utxJfBY-D7A1smdFhSnIA; csrftoken=GcIusuHLJOCxMHiZ1wbI8IsqkcybWq2t; _ga=GA1.3.309538033.1473872047; pl_profile="eyJzIjogIld6SXNNemc1TkRjME5GMDoxY05MRlY6TFE0aE0yWTI4WFhOaTdqeW04c1lqdXU2Nk4wIiwgInUiOiB7ImxuIjogIkJhcGF0IiwgImZjIjogOCwgImlkIjogMzg5NDc0NCwgImZuIjogIkNoYWl0YW55YSJ9fQ=="; sessionid=".eJyrVkpPzE2NT85PSVWyUirISSvIUdJRik8sLcmILy1OLYpPSkzOTs1LAUsmVqYW6UEFivUCwHwnqDyKpkyg-mhDHWMLSxNzE5PYWgBVoiN7:1cNcq5:gyTfZ4HaGrHPDIPnYJh0O6NGLKs"; _gat=1; _dc_gtm_UA-33785302-1=1' -H 'Connection: keep-alive'

Here actual Sessions, parameters, tokens were passed





Traceback (most recent call last):
  File "user.py", line 23, in <module>
    json_data = json.loads(response.text)
  File "/usr/lib64/python2.7/json/__init__.py", line 339, in loads
    return _default_decoder.decode(s)
  File "/usr/lib64/python2.7/json/decoder.py", line 364, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
  File "/usr/lib64/python2.7/json/decoder.py", line 382, in raw_decode
    raise ValueError("No JSON object could be decoded")
ValueError: No JSON object could be decoded

Solution - Handle the exception (basically bad request)


File "user.py", line 36, in <module>
    response = requests.get("https://fantasy.premierleague.com/drf/entry/"+str(counter))
  File "/usr/lib/python2.7/site-packages/requests/api.py", line 71, in get
    return request('get', url, params=params, **kwargs)
  File "/usr/lib/python2.7/site-packages/requests/api.py", line 57, in request
    return session.request(method=method, url=url, **kwargs)
  File "/usr/lib/python2.7/site-packages/requests/sessions.py", line 475, in request
    resp = self.send(prep, **send_kwargs)
  File "/usr/lib/python2.7/site-packages/requests/sessions.py", line 585, in send
    r = adapter.send(request, **kwargs)
  File "/usr/lib/python2.7/site-packages/requests/adapters.py", line 403, in send
    timeout=timeout
  File "/usr/lib/python2.7/site-packages/requests/packages/urllib3/connectionpool.py", line 578, in urlopen
    chunked=chunked)
  File "/usr/lib/python2.7/site-packages/requests/packages/urllib3/connectionpool.py", line 385, in _make_request
    httplib_response = conn.getresponse(buffering=True)
  File "/usr/lib64/python2.7/httplib.py", line 1136, in getresponse
    response.begin()
  File "/usr/lib64/python2.7/httplib.py", line 453, in begin
    version, status, reason = self._read_status()
  File "/usr/lib64/python2.7/httplib.py", line 409, in _read_status
    line = self.fp.readline(_MAXLINE + 1)
  File "/usr/lib64/python2.7/socket.py", line 480, in readline
    data = self._sock.recv(self._rbufsize)
  File "/usr/lib64/python2.7/ssl.py", line 756, in recv
    return self.read(buflen)
  File "/usr/lib64/python2.7/ssl.py", line 643, in read
    v = self._sslobj.read(len)
    




1. dnf install mongodb mongodb-server
Installed:
  boost-program-options.x86_64 1.60.0-10.fc25                                   
  boost-regex.x86_64 1.60.0-10.fc25                                             
  mongodb.x86_64 3.2.8-2.fc25                                                   
  mongodb-server.x86_64 3.2.8-2.fc25                                            
  mozjs38.x86_64 38.8.0-2.fc25                                                  
  yaml-cpp.x86_64 0.5.1-13.fc24     
  
2.systemctl start mongod
3.
[root@chai chai]# mongo
MongoDB shell version: 3.2.8
connecting to: test
  
use fpl_users;
switched to db fpl_users
> db
fpl_users
> db.createCollection("fpl_user_data");
{ "ok" : 1 }

show collections
fpl_user_data



mongoimport -d fpl_users -c fpl_user_data --type csv --file data.csv --headerline
bash: mongoimport: command not found...
Packages providing this file are:
'mongodb-org-tools'
'mongo-tools'

dnf install mongodb-org-tools
Failed to synchronize cache for repo 'dockerrepo', disabling.
Last metadata expiration check: 0:27:04 ago on Thu Jan  5 11:01:05 2017.
Error: package mongodb-org-tools-3.2.0-1.el7.x86_64 conflicts with mongodb-server provided by mongodb-server-3.2.8-2.fc25.x86_64
(try to add '--allowerasing' to command line to replace conflicting packages)

Solution
dnf install mongo-tools

Absolute path - data.csv
mongoimport -d fpl_users -c fpl_user_data --type csv --file data.csv --headerline
2017-01-05T11:54:03.753+0530	connected to: localhost
2017-01-05T11:54:04.338+0530	imported 19214 documents

All CSV files imported similarly


db.fpl_user_data.find().pretty()

Problem - 
 db.fpl_user_data.find().sort({KEY:1})
Error: error: {
	"waitedMS" : NumberLong(0),
	"ok" : 0,
	"errmsg" : "Executor error during find command: OperationFailed: Sort operation used more than the maximum 33554432 bytes of RAM. Add an index, or specify a smaller limit.",
	"code" : 96
}

Solution    
http://pe-kay.blogspot.in/2016/05/how-to-change-mongodbs-sort-buffer-size.html

db.adminCommand({"setParameter":1,"internalQueryExecMaxBlockingSortBytes":134217728})
{ "was" : 33554432, "ok" : 1 }
> db.fpl_user_data.find().sort({KEY:1})


db.fpl_user_data.find().sort({KEY:"main_user_id"})
Error: error: {
	"waitedMS" : NumberLong(0),
	"ok" : 0,
	"errmsg" : "bad sort specification",
	"code" : 2
}


Issue - Output was not coming in sorted fashion

Solution
db.collection_name.find().sort({key:1}).pretty()

db.fpl_user_data.find().sort( { main_user_id: 1 } ).pretty()


Problem - Accesing MongoDB using Python
https://docs.mongodb.com/getting-started/python/client/
https://docs.mongodb.com/getting-started/python/query/

Aggregation
https://docs.mongodb.com/getting-started/python/aggregation/

Data inconsistency
1.Incomplete
count - 193212
last elemet id 198181
2.empty data fields (ASCII Unicode)
first name last name
main user team name

3.duplicate data
Solution
>db.collection-name.remove({'key':'value'})
key = attribute name / field / column name
value = corresponding value


errors
1.
('Connection aborted.', error(22, 'Invalid argument'))
2.
HTTPSConnectionPool(host='fantasy.premierleague.com', port=443): Max retries exceeded with url: /drf/entry/2365060 (Caused by NewConnectionError('<requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f9b6dcf1e50>: Failed to establish a new connection: [Errno 110] Connection timed out',))

3.
No JSON object could be decoded