| First Column | Second Column |
| --- | --- |
| Row 1 | First row, first code block, and have a Java code block with some code, like this

```java
public String getCurrentURI()
{
    if (_currentFree == 0)
    {
        return null;
    }
    if (_current >= _currentFree)
    {
        _current = 0;
    }
    return _uris[_current];
}
```

First row, second code block:

```
<Options> <Topic>legacy:new</Topic> </Options>
```

First row, third block:

```java
public int getConnectWaitDuration(String uri_) throws Exception
``` |
| Row 2 | Second row content | 
