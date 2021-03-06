title: Curl
author: Matthew De Goes
date: 2013-03-26 12:20
template: page-devcntr.jade

<div>
<a name="curl" id="curl"></a>

<h2>ACCESSING USING CURL</h2>

<p>You can use the curl command from the terminal to call the API. Information about curl is available at <a href="http://curl.haxx.se/docs/manpage.html">http://curl.haxx.se/docs/manpage.html</a>.</p>

<p>Here are some example API calls using curl. If no content type is passed, the default is: "application/json".</p>

<ul>
    <li>
        <p>Create an account:</p>
        <pre>
curl -X POST "https://beta.precog.com/accounts/v1/accounts/" -d '{"email":"yourEmailAddress", "password": "yourSecurePassword"}' -H 'Content-Type: application/json' 
</pre>
    </li>

    <li>
        <p>Describe your account where <span class="tool-tip-account-id">'accountId</span> is your accountId number returned from the create an account API call:</p>
        <pre>
curl "https://beta.precog.com/accounts/v1/accounts/<span class="tool-tip-account-id">'accountId</span>" -u "yourEmailAddress:yourSecurePassword" -H 'Content-Type: application/json' 
</pre>
    </li>

    <li>
        <p>Ingest a file called "data" formatted as one json object per line to /101015555/test/curl/example</p>
        <pre>
curl -X POST "https://beta.precog.com/ingest/v1/sync/fs/101015555/test/curl/example?apiKey=[authorizing API Key]" --data-binary @data.json  -H 'Content-Type: application/json' 
</pre>

        <p>Note the use of --data-binary in this example instead of just -d; this syntax is necessary to communicate to curl that you want preserve new-line characters. Without new-line characters, the file will not be parsed appropriately.</p>
    </li>

    <li>
        <p>Ingest a file called "tables" in CSV format to /101015555/csv/test</p>
        <pre>
curl -X POST "https://beta.precog.com/ingest/v1/sync/fs/101015555/csv/test?apiKey=[authorizing API Key]" --data-binary @tables.csv  -H 'Content-Type: text/csv' 
</pre>

        <p>Note the use of --data-binary in this example instead of just -d; this syntax is necessary to communicate to curl that you want preserve new-line characters. Without new-line characters, the file will not be parsed appropriately.</p>
    </li>

    <li>
        <p>Delete the path: /101015555/csv/test</p>
        <pre>
curl -X DELETE "https://beta.precog.com/ingest/v1/sync/fs/101015555/csv/test?apiKey=[authorizing API Key]" 
</pre>
    </li>

    <li>
        <p>Run a query that returns transactions data that have greater than the average price. Note that the query needs to be URL encoded and thus all the spaces have been replaced with "+". The API Key used in this example has read-only access to this path and the unaltered code should return results.</p>
        <pre>
curl "https://beta.precog.com/analytics/v1/fs/0000000289?q=data+:=+//tutorial/transactions+data+where+data.price+&gt;+mean(data.price)&amp;apiKey=ACA34385-B8F7-493A-835D-C8C310C45A16" -v 
</pre>
    </li>

    <li>
        <p>Create an API Key that provides a read grant to path /0000000289/tutorial/transactions/ using an API Key that also has read-only access to that path.</p>
        <pre>
curl -X POST "https://beta.precog.com/security/v1/apikeys/?apiKey=ACA34385-B8F7-493A-835D-C8C310C45A16" -d '{ "name": "read only 1", "description": "read only for dashboard", "grants": [ {"name": "AP read only", "description": "apocalypse products dashboard", "parentIds": [], "permissions": [{"accessType": "read", "path": "/0000000289/tutorial/transactions/", "ownerAccountIds": ["0000000289"]}], "expirationDate": null}] }' -v
</pre>
    </li>
</ul>
</div>