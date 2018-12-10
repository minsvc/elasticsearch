 #!/bin/bash

  # The script performs force relocation of all unassigned shards,
  # of all indices to a specified node (NODE variable)

  ES_HOST="http://localhost:9200"
  TARGET_NODE="hot-1"
  SRC_NODE="hot-3"

  curl ${ES_HOST}/_cat/shards > shards
  grep "hot-3" shards > unassigned_shards

  # Now move all shard in the unnasigned_shards file onto the target node
  while read LINE; do
    IFS=" " read -r -a ARRAY <<< "$LINE"
    INDEX=${ARRAY[0]}
    SHARD=${ARRAY[1]}

    echo "Relocating:"
    echo "Index: ${INDEX}"
    echo "Shard: ${SHARD}"
    echo "To node: ${TARGET_NODE}"

    curl -s -XPOST "${ES_HOST}/_cluster/reroute?pretty" -d "{
      \"commands\": [
         {
           \"move\": {
             \"index\": \"${INDEX}\",
             \"shard\": ${SHARD},
             \"to_node\": \"${TARGET_NODE}\",
             \"from_node\": \"${SRC_NODE}\"
           }
         }
       ]
    }"; echo
    echo "------------------------------"
  done <unassigned_shards

  exit 0
