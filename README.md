# elasticsearch 5.6 test pass

sometimes we need to scale down elasticsearch cluster to one node

## 1.shutdown the replicas to save the disk space

curl -X PUT "localhost:9200/_all/_settings" -H 'Content-Type: application/json' -d'
{
    "number_of_replicas": 0
}
'
### check:
curl -XGET 'http://localhost:9200/_all/_settings/*?pretty' 2> /dev/null | grep number_of_replicas 

## 2.shutdown the shard allocation

curl -XPUT http://localhost:9200/_cluster/settings -d '{
    "transient" : {
        "cluster.routing.allocation.enable" : "none"
    }
}'

### check: 
curl -X GET "localhost:9200/_cluster/settings?include_defaults=true&pretty" 2>/dev/null | grep transient -B10 -A 10

## 3.use migration.sh reroute primary shard to target node

change the scr node and target node you want,this meaning all the primary shard of node "hot-3' reroute to "hot-1" 
TARGET_NODE="hot-1"
SRC_NODE="hot-3"


