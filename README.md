# Scrapy Cluster


### Steps to launch the test environment:
1. Build your containers (or omit --build to pull from docker hub)
```
docker-compose up -d --build
```
2. Tail kafka to view your future results
```
docker-compose exec kafka_monitor python kafkadump.py dump -t demo.crawled_firehose -ll INFO
```
3. From another terminal, feed a request to kafka
```
curl localhost:5343/feed -H "content-type:application/json" -d '{"url": "http://dmoztools.net", "appid":"testapp", "crawlid":"abc123"}'
```
4. Validate you've got data!
```
# wait a couple seconds, your terminal from step 2 should dump json data
{u'body': '...content...', u'crawlid': u'abc123', u'links': [], u'encoding': u'utf-8', u'url': u'http://dmoztools.net', u'status_code': 200, u'status_msg': u'OK', u'response_url': u'http://dmoztools.net', u'request_headers': {u'Accept-Language': [u'en'], u'Accept-Encoding': [u'gzip,deflate'], u'Accept': [u'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8'], u'User-Agent': [u'Scrapy/1.5.0 (+https://scrapy.org)']}, u'response_headers': {u'X-Amz-Cf-Pop': [u'IAD79-C3'], u'Via': [u'1.1 82c27f654a5635aeb67d519456516244.cloudfront.net (CloudFront)'], u'X-Cache': [u'RefreshHit from cloudfront'], u'Vary': [u'Accept-Encoding'], u'Server': [u'AmazonS3'], u'Last-Modified': [u'Mon, 20 Mar 2017 16:43:41 GMT'], u'Etag': [u'"cf6b76618b6f31cdec61181251aa39b7"'], u'X-Amz-Cf-Id': [u'y7MqDCLdBRu0UANgt4KOc6m3pKaCqsZP3U3ZgIuxMAJxoml2HTPs_Q=='], u'Date': [u'Tue, 22 Dec 2020 21:37:05 GMT'], u'Content-Type': [u'text/html']}, u'timestamp': u'2020-12-22T21:37:04.736926', u'attrs': None, u'appid': u'testapp'}
```
