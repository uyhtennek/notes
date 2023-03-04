### Missing array item
Servers use path to find data, so it is somewhat like a bunch of key-value pairs. Surely they have no problem storing Dictionary, but Array will look a bit strange on server like Firebase. You can see it includes a key for a array, even though array doesn't use key.

![[Firebase-array.png]]
In this example, ``ex_expeditionsOrder`` is a array ``[ 23, 11, 10, 14, 13, 16 ]``. In Firebase, we can export this item and then it will return a json containing ``[ 23, 11, 10, 14, 13, 16 ]`` just fine.

![[Firebase-missing-array-item.png]]
However, if we delete an item from the start or middle. Then the array in the returned json becomes ``[ 23, 11, 10, 14, null, 16 ]``. Obviously if our code didn't checkout for null then it will give an error most likely.

So either be sure there will never be a missing item, or check for null in the code.
