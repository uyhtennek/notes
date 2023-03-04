### Array or Dictionary?
In my opinion, the key is whether the item is designed to be repeatable or not.

The tricky part to this question is: sometimes you only need a array for your code but it is more safe to make it a dictionary if the items are not allow to be repeating. This is more true when this piece of data is going to a server. Plus, we won't encounter the problem of [[Missing array item]] if we use dictionary on server.

For example, you have a array ``[ 1, 2, 3, 4 ]`` which contains mission ID and the index number is their rendering order. As they are ID so we don't want any one of these items repeated. If the data is like ``[ 1, 2, 2, 3, 4 ]``, the code fails.

To avoid this, we make the whole item a dictionary. Then make the ID as key, and the order number as value.
``` json
{
  "missionID-1" : "1",
  "missionID-2" : "2",
  "missionID-3" : "3",
  "missionID-4" : "4",
}
```
Because dictionary is not allowed to have repeating keys, there won't be another ``"missionID-2"``. The value could be repeating to others value though, but at least it will not make the data size, thus memory usage and even server usage, increase.