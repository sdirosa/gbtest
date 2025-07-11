# Table test

AMPS provides the ability for incoming commands to be modified, or _filtered_.

Making changes

When one or more transport filters are specified, AMPS provides each incoming command to those filters as soon as the header for the message is parsed. Each filter can modify the message data or a subset of the headers, and can choose to have AMPS stop processing the command (or can request that AMPS disconnect the connection that submitted the command).

The filters for a `Transport`, if any, are defined in the configuration for the transport. When more than one filter is specified, AMPS runs each filter in the order in which they appear in the configuration file.

Transport filters are implemented as extension modules. To create an extension module, contact AMPS support for the server SDK.

The following table doesn't really contain the most accurate since it is solely for testing purposes.

An internal function used to check if the configured retry has been exceeded, and if so, throw MaximumRetryExceeded to cause the calling HAClient to give up on reconnecting to a server.



AMPS loads the following transport filters by default:

| Module                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amps-topic-translator`           | <p>Translates topic names on incoming commands.</p><p>This module requires one or more of the following options: <code>Topic</code></p><p>Specifies the translation to use. This option takes the following format:<br><em>original</em> <code>:</code> <em>translated</em></p><p></p><pre><code> public String getCurrentURI()
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
</code></pre><p>The <em>original</em> parameter can be a literal topic name or a PCRE regex. Any topic on any command that matches that parameter will be converted to the translated topic.</p><p>For example, to convert the topic <code>legacy</code> to the topic <code>new</code>, you would specify the following option:</p><pre><code> &#x3C;Options> &#x3C;Topic>legacy:new&#x3C;/Topic> &#x3C;/Options> 
</code></pre><p>To translate any topic beginning with <code>/orders/northamerica</code> to <code>NAorders</code>, you would specify the following option:  </p><p></p><p>Gets the connection wait duration based on the provided URI:</p><pre><code><strong>public int getConnectWaitDuration(String uri_) throws Exception
</strong>    {
        _throwIfMaximumExceeded();
        if(_lastUri == null || !_lastUri.equals(uri_))
        {
            _lastUri = uri_;
            if(_firstUri != null &#x26;&#x26; _firstUri.equals(uri_))
            {
                // we've wrapped around the "list." Delay.
                return _currentDurationAndIncrease();
            }
            else if(_firstUri == null)
            {
                _firstUri = uri_;
            }
            return 0;
        }
        return _currentDurationAndIncrease();
    }
</code></pre><p></p><p>Options and DefaultServerChooser Code: </p><p>Have a condition.</p><pre><code> &#x3C;Options> 
 
 &#x3C;Topic>^/orders/northamerica:NAorders&#x3C;/Topic> 
 
 &#x3C;/Options> 

public DefaultServerChooser add(String uri)
    {
        if (_currentFree >= _uris.length)
        {
            String[] newUris = new String[_uris.length * 2];
            System.arraycopy(_uris, 0, newUris, 0, _uris.length);
            _uris = newUris;
        }

        _uris[_currentFree] = uri;
        ++_currentFree;
        return this;
    }
</code></pre><p></p><p>Gets the connection wait duration based on the provided URI. </p><pre><code>public &#x3C;T extends Collection&#x3C;String> > DefaultServerChooser addAll(T uris)
    {
        int urisSize = uris.size();
        if (_currentFree >= _uris.length - urisSize)
        {
            int newLength = (_uris.length&#x3C;urisSize)?_uris.length+urisSize:_uris.length*2;
            String[] newUris = new String[newLength];
            System.arraycopy(_uris, 0, newUris, 0, _uris.length);
            _uris = newUris;
        }

        for (String uri : uris)
        {
            _uris[_currentFree] = uri;
            ++_currentFree;
        }
        return this;
    }
</code></pre> |
| `amps-conflated-topic-translator` | <p>Translates an incoming <code>subscribe</code>, <code>sow_and_subscribe</code>, <code>delta_subscribe</code>, or <code>sow_and_delta_subscribe</code> command for a specific topic name as follows:</p><ul><li>Translates the topic name on the command to a different topic name.</li><li>Adds a conflation interval to the command, if there is no conflation interval specified on the incoming command.</li></ul><p>This module can be useful for removing a conflated topic that is infrequently-used, or for which subscribers only monitor a small number of messages out of the overall topic.</p><p>This module requires one or more of the following options: <code>Topic</code></p><p>Specifies the translation to use. This option takes the following format:<br><em>original</em> <code>:</code> <em>translated</em> <code>:</code> <em>interval</em></p><p>The <em>interval</em> specifies the conflation interval to apply to the translated commands if one is not provided.</p><p>For example, to convert all subscriptions to the topic <code>orders-C</code> to the topic <code>orders</code>, and guarantee that each translated subscription has a conflation specified, with a 500 millisecond default for conflation, you would use the following options:</p><pre><code> &#x3C;Options> &#x3C;Topic>orders-C:orders:500ms&#x3C;/Topic> &#x3C;/Options> 
</code></pre><p>To convert all subscriptions to the topics <code>slowUpdates</code> and <code>verySlowUpdates</code> to the topic <code>updates</code> and guarantee that each translated subscription has a conflation specified, with a 2 second default for conflation, you would use the following options:</p><pre><code> &#x3C;Options> &#x3C;Topic>slowUpdates:updates:2s&#x3C;/Topic> &#x3C;Topic>verySlowUpdates:updates:2s&#x3C;/Topic> &#x3C;/Options> 
</code></pre>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
