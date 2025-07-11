# Page



| Module                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amps-topic-translator`           | <p>Row 1 code block 1:</p><pre><code> public String getCurrentURI()
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
</code></pre><p>Row 1 code block 2:</p><pre><code> &#x3C;Options> &#x3C;Topic>legacy:new&#x3C;/Topic> &#x3C;/Options> 
</code></pre><p>Row 1 code block 3:</p><pre><code><strong>public int getConnectWaitDuration(String uri_) throws Exception
</strong></code></pre> |
| `amps-conflated-topic-translator` | <p>Row 2 code block 1:</p><pre><code> &#x3C;Options> &#x3C;Topic>orders-C:orders:500ms&#x3C;/Topic> &#x3C;/Options> 
</code></pre><p>Row 2 code block 2:</p><pre><code> &#x3C;Options> &#x3C;Topic>slowUpdates:updates:2s&#x3C;/Topic> &#x3C;Topic>verySlowUpdates:updates:2s&#x3C;/Topic> &#x3C;/Options> 
</code></pre>                                                                                                                                                                                                                               |
